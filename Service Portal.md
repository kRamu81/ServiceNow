# ServiceNow Service Portal: A Complete Practical and Conceptual Guide

> Compiled with reference to ServiceNow Product Documentation, the ServiceNow Community and the ServiceNow Developer Portal.

---

## Table of Contents

- [Introduction](#introduction)
- [1. What is Service Portal?](#1-what-is-service-portal)
  - [Key characteristics](#key-characteristics)
- [2. Use and Benefits of Service Portal](#2-use-and-benefits-of-service-portal)
  - [Primary uses](#primary-uses)
  - [Benefits](#benefits)
- [3. Service Portal vs Employee Center (ESC)](#3-service-portal-vs-employee-center-esc)
- [4. Portal, Page, Container, Row and Column](#4-portal-page-container-row-and-column)
  - [How it fits together](#how-it-fits-together)
  - [Scenario: Building a homepage](#scenario-building-a-homepage)
- [5. Widgets: Architecture and Lifecycle](#5-widgets-architecture-and-lifecycle)
  - [Technical definition](#technical-definition)
  - [Widget architecture: the 5 parts](#widget-architecture-the-5-parts)
  - [Widget lifecycle](#widget-lifecycle-typical-page-load)
  - [Backend tables for widgets](#backend-tables-for-widgets)
  - [Scenario: My Open Tickets widget](#scenario-my-open-tickets-widget)
- [6. ng-Directives in Service Portal](#6-ng-directives-in-service-portal)
- [7. Limitations, Benefits and Best Practices](#7-limitations-benefits-and-best-practices)
  - [Limitations](#limitations)
  - [Benefits (recap)](#benefits-recap)
  - [Best practices](#best-practices)
- [References](#references)

---

## Introduction

The **Service Portal** is ServiceNow's framework for building responsive, branded, self-service websites on top of the Now Platform. It is the layer end users actually see and click through (raising a ticket, browsing the catalog, tracking a request, or reading a knowledge article) without ever touching the native ServiceNow UI (UI16 / Workspace forms).

This guide walks through the core concepts, the underlying data model, the widget lifecycle, and practical best practices.

[Back to top](#table-of-contents)

---

## 1. What is Service Portal?

Service Portal is a Now Platform framework, introduced in the **Fuji** release, that lets developers and administrators build modern, mobile-responsive, drag-and-drop websites ("portals") for employees, customers, or partners, without needing to write an entire application from scratch.

It is built on **AngularJS 1.x**, **Bootstrap**, and server-side **Glide APIs** running on the **Rhino** JavaScript engine, and it renders as a single-page application at a URL such as `https://<instance>.servicenow.com/sp`.

Out of the box, ServiceNow ships several ready-made portals:

- Service Portal (`/sp`)
- Customer Service Portal (`/csm`)
- Employee Center (`/esc`), a separate, newer product

Administrators can also design brand-new portals for HR, Facilities, Legal, IT, or any department that needs a self-service front end.

### Key characteristics

- Built from re-usable, drag-and-drop **widgets** arranged on **pages**.
- Fully responsive (mobile, tablet, desktop) using the Bootstrap grid.
- Themeable/brandable: logo, colors, CSS variables per portal.
- Configurable without coding via the **Service Portal Designer**, and extensible with code (HTML, AngularJS, server-side JavaScript) for custom widgets.
- Supports role-based access, multiple languages, and can be scoped to specific user criteria.

[Back to top](#table-of-contents)

---

## 2. Use and Benefits of Service Portal

### Primary uses

- Self-service IT service catalog: browse and order catalog items, software, hardware.
- Logging and tracking incidents and requests without opening the agent workspace.
- Knowledge base search and article consumption.
- Building department-specific portals (HR, Facilities, Legal, Finance) using the same engine.
- Approvals, surveys, announcements, and dashboards for end users.

### Benefits

| Benefit | Why it matters |
|---|---|
| Improved user experience | Modern, responsive UI reduces training needs and increases self-service adoption. |
| Reduced call/ticket volume | Users solve problems themselves via catalog and knowledge search, cutting Service Desk load. |
| Reusable widgets | One widget can be dropped on many pages/portals, speeding up delivery. |
| Low-code configuration | Page/Portal layout is drag-and-drop; only custom widgets need scripting. |
| Brand consistency | Theme and CSS variables let each portal reflect a department's or company's branding. |
| Extensible | Full access to AngularJS + GlideRecord/GlideAjax lets developers build almost anything. |
| Mobile-friendly | Bootstrap-based responsive grid works across devices without separate mobile builds. |

[Back to top](#table-of-contents)

---

## 3. Service Portal vs Employee Center (ESC)

**Employee Center (ESC)** is ServiceNow's newer, purpose-built employee experience portal, based on the **Now Experience UI framework** (Web Components + UI Builder) rather than AngularJS/Service Portal. It is designed to unify HR, IT, Facilities, and other department services into a single, personalized front door for employees, and it is where ServiceNow is investing most new self-service innovation.

| Aspect | Service Portal | Employee Center (ESC) |
|---|---|---|
| Underlying tech | AngularJS 1.x, Bootstrap, Rhino/server scripts | Now Experience UI Framework (Web Components), UI Builder |
| Audience | Employees, customers, partners (general purpose) | Primarily employees; a single unified workplace experience |
| Cross-department view | Usually separate portals per department | Single portal aggregating IT, HR, Facilities, Finance, etc. |
| Personalization | Manual / custom-built per portal | Built-in role-, department-, location-based personalization |
| Search | Scoped to what's configured in that portal | Unified search across knowledge, catalog, requests |
| Customization depth | Highly flexible: full widget/HTML/CSS/JS control | Configured mainly via UI Builder; less low-level code access |
| Building blocks | Portal → Page → Container → Row → Column → Widget | Experience → Page → Component (via UI Builder) |
| Roadmap | Maintained, but not the primary innovation focus | Active investment: new AI/GenAI features land here first |

**In short:** Service Portal is the flexible, code-friendly, general-purpose portal framework; Employee Center is the modern, low-code, unified employee-experience product built on the newer Now Experience framework.

[Back to top](#table-of-contents)

---

## 4. Portal, Page, Container, Row and Column

A Service Portal is assembled from a strict layout hierarchy, mirroring the Bootstrap grid system. Understanding this hierarchy is essential for building or debugging any page.

| Layer | What it is | Backend table |
|---|---|---|
| **Portal** | The overall site: defines URL suffix, theme, home page, header/footer, login page, and access rules. | `sp_portal` |
| **Page** | A single screen/route inside a portal (e.g. Homepage, Catalog, Ticket view). A page belongs to one or more portals. | `sp_page` |
| **Container** | The outermost horizontal band on a page, holds one or more Rows. Can be full-width or fixed-width, with its own background/CSS. | `sp_container` |
| **Row** | A horizontal strip inside a container, holding Columns. Rows stack vertically inside their container. | `sp_row` |
| **Column** | A vertical slice of a Row, sized on a 12-unit Bootstrap grid (e.g. `col-md-6` = half width). Columns hold Widget Instances. | `sp_column` |
| **Widget Instance** | A specific placement of a widget inside a column, with its own options/config for that placement. | `sp_instance` / `sp_instance_2` |

### How it fits together

```
Portal → Page → Container → Row → Column → Widget Instance → Widget
```

A container can hold multiple rows; each row is split into columns (up to 12 grid units wide in total, Bootstrap style); each column can hold one or more widget instances. This is exactly what you manipulate visually in the drag-and-drop **Service Portal Designer**.

### Scenario: Building a homepage

An admin creates a Page called `homepage`, adds a full-width Container for the banner, a Row with 3 Columns (4-4-4 split) each holding a "Quick Link" widget instance, and a second Row with a single full-width Column holding the "Knowledge Search" widget. Every element the admin drags is a live record in `sp_container`, `sp_row`, `sp_column` and `sp_instance`, all linked back to the `sp_page` record.

[Back to top](#table-of-contents)

---

## 5. Widgets: Architecture and Lifecycle

A **widget** is the fundamental building block of Service Portal, essentially a self-contained mini-application. It is a superset of an AngularJS directive that is tightly coupled to a server-side JavaScript block powered by the Rhino engine. Almost everything visible on a portal page (banners, forms, lists, the Service Catalog, Knowledge Search) is implemented as a widget.

### Technical definition

> A widget is a self-contained, reusable component in Service Portal that combines server-side JavaScript, client-side AngularJS, HTML, and CSS to render content and handle user interactions on a portal page.

### Widget architecture: the 5 parts

| Part | Purpose | Runs where |
|---|---|---|
| **HTML Template** | Defines the markup/layout using AngularJS bindings (`{{ }}`, `ng-if`, `ng-repeat`, etc.) | Client (browser) |
| **Client Script (Controller)** | AngularJS controller logic: handles clicks, calls the server via `c.server.get()`, manages `$scope` / `c.data`. | Client (browser) |
| **Server Script** | GlideRecord/Glide API logic that prepares the `data` object before the page renders (and answers AJAX calls). | Server (Rhino) |
| **CSS (SCSS)** | Styling scoped to the widget instance. | Client (browser) |
| **Option Schema / Link function** | Defines configurable options exposed per widget instance; the (optional) Link function runs after the DOM is ready for direct DOM manipulation. | Client (browser) |

### Widget lifecycle (typical page load)

1. **Server Script executes first.** For every widget instance on the page, the server script runs on the server and builds the initial `data` object (this happens server-side, before any HTML reaches the browser).
2. **HTML + server data are sent to the browser.** The rendered widget instance (HTML template + initial data as JSON) is delivered as part of the page payload.
3. **AngularJS bootstraps and the Client Controller runs.** `c.data` is initialized from the server's `data` object; the controller wires up `$scope`, event handlers, and any watchers.
4. **HTML Template data-binds.** The template renders using `c.data` / `$scope` values; directives such as `ng-repeat` and `ng-if` evaluate.
5. **(Optional) Link function runs.** Once the DOM for the widget is compiled and linked, for any direct DOM/jQuery manipulation.
6. **Client ↔ Server round-trips.** Subsequent user actions call `c.server.get()` (via GlideAjax under the hood) to re-run the server script and refresh `c.data` without a full page reload.

This confirms the common developer question on the ServiceNow Community: yes, the server script always executes before the client controller on initial load, and data set on the server `data` object is directly available in the HTML template through `data.xxx` or, after the controller assigns it, through `c.data.xxx`.

### Backend tables for widgets

| Table | Stores |
|---|---|
| `sp_widget` | The widget definition itself: id, HTML template, server script, client script (controller), CSS, option schema, dependencies. |
| `sp_instance` / `sp_instance_2` | A specific placement (instance) of a widget on a column: title, options/config values, order. |
| `sp_ng_template` | Additional Angular ng-templates associated with a widget (used with `ng-include`). |
| `sp_widget_dependency` / `sp_dependency` | JS/CSS libraries the widget depends on. |
| `sp_angular_provider` | Custom AngularJS directives, filters, or services registered for use inside widgets. |
| `sp_widget_annotation` | Notes/comments attached to widget development. |

### Scenario: My Open Tickets widget

- The **Server Script** runs a GlideRecord query on the `incident` table filtered to `caller_id = current user` and pushes the results into `data.tickets`.
- The **HTML template** loops over them with `ng-repeat="ticket in c.data.tickets"`.
- Clicking a row calls a **Client Controller** function that uses `$location` to route to the ticket page, passing the `sys_id` as a URL parameter. This is exactly the pattern used by the OOB "Service Portal related list" widget.

[Back to top](#table-of-contents)

---

## 6. ng-Directives in Service Portal

Because widget HTML templates run inside AngularJS 1.x, Service Portal developers rely heavily on Angular's built-in directives ("ng-" prefixed attributes) to bind data, loop over arrays, and add conditional/interactive behavior, without writing imperative DOM code.

| Directive | Why / Where it's used |
|---|---|
| `ng-repeat` | Loops over an array (e.g. a list of incidents) to render one DOM element per item. Used constantly for lists, tables, cards. |
| `ng-if` | Conditionally adds/removes an element from the DOM (preferred over `ng-show`/`ng-hide` when the element should not exist at all, for performance/security). |
| `ng-show` / `ng-hide` | Toggles CSS display without removing the element from the DOM. Used for quick show/hide toggles. |
| `ng-model` | Two-way binds a form input (text box, checkbox, select) to a controller variable, e.g. a search box bound to `c.data.searchTerm`. |
| `ng-click` | Binds a click event to a controller function, e.g. submitting a form or navigating to another page. |
| `ng-class` | Dynamically applies CSS classes based on an expression, e.g. highlighting a priority-1 incident row in red. |
| `ng-bind` / `ng-bind-html` | Binds text (or, cautiously, trusted HTML) content into an element; safer than `{{ }}` in some contexts. |
| `ng-include` | Pulls in a separate ng-template (stored in `sp_ng_template`) to keep large widgets modular and maintainable. |
| `ng-change` | Fires when a bound model's value changes (e.g. auto-searching as a user types, often combined with `ng-model-options` debounce). |
| `ng-src` / `ng-style` | Dynamically sets an image source or inline style from a scope expression, avoiding broken/invalid attribute values before Angular compiles. |

Developers can also register **custom AngularJS directives** (via the `sp_angular_provider` table / Angular Providers module) to create reusable, hyphenated custom tags (e.g. a camelCase directive `spButton` becomes `<sp-button>` in HTML) that can be shared across multiple widgets.

[Back to top](#table-of-contents)

---

## 7. Limitations, Benefits and Best Practices

### Limitations

- Built on **AngularJS 1.x**, a framework Google has deprecated. No new Angular-side features are being added by ServiceNow, and its roadmap is shifting toward the Now Experience framework (Employee Center / UI Builder).
- **Update Set migrations are fragile:** the Designer often captures only what you directly changed, so related child records (containers, rows, columns, widget instances) can be missed, breaking the page in the target instance (a frequently reported issue on the ServiceNow Community).
- **Out-of-box widgets are read-only.** To change their behavior you must clone them, which creates maintenance overhead and can diverge from future OOB updates.
- No built-in cross-department personalization or unified search the way Employee Center offers.
- **Performance** can degrade with many widgets/watchers on a single page due to AngularJS digest-cycle overhead.
- Requires developers to know AngularJS, Bootstrap, and Glide APIs, a wider skill set than pure no-code tools.

### Benefits (recap)

- Rapid, low-code page assembly via drag-and-drop Designer.
- Highly reusable, shareable widgets across portals.
- Deep customization potential via HTML/CSS/AngularJS/server scripts.
- Mature, well-documented platform with a large community of examples (ServicePortal.io, ServiceNow Community, Developer Blog).

### Best practices

- **Clone, don't edit** out-of-box widgets; give clones a clear naming convention (e.g. prefix with a scoped app id).
- Prefer `ng-if` over `ng-show`/`ng-hide` when the element and its watchers truly aren't needed, to reduce DOM/watcher overhead.
- Keep server scripts lean: query only the fields you need, use `GlideRecord.setLimit()`, and avoid heavy logic in widgets that load on every page.
- Use `c.server.get()` for on-demand data refreshes instead of reloading the whole page.
- Modularize large widgets with `ng-include` + ng-template (`sp_ng_template`) rather than one giant HTML block.
- Always include the **full record hierarchy** (page, container, row, column, widget instance) in Update Sets, or migrate via XML export as a fallback.
- Follow the official Service Portal Style Guide / Development Guidelines published on the ServiceNow Community for naming, structure, and accessibility.
- Use **Option Schema** fields to make widgets configurable per instance instead of hardcoding values.
- Test responsiveness across breakpoints (Bootstrap's `col-xs` / `sm` / `md` / `lg`) since portals must work on mobile.
- Where the roadmap allows, evaluate Employee Center / UI Builder for new, employee-facing initiatives, reserving Service Portal for scenarios needing deep custom control.

[Back to top](#table-of-contents)

---

## References

- **ServiceNow Product Documentation**: docs.servicenow.com (Platform User Interface / Service Portal, Employee Center)
- **ServiceNow Community**: Developer Forum and Developer Blog (Service Portal widget lifecycle, ng-repeat/ng-if articles, Update Set migration threads, Service Portal Development Guidelines)
- **ServicePortal.io**: independent Service Portal documentation and widget reference by a former ServiceNow Service Portal engineering lead
- **ServiceNow Developer Portal**: developer.servicenow.com (widget templates, guided widget examples)

[Back to top](#table-of-contents)
