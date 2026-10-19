# ServiceNow ITAM (IT Asset Management) Notes

*Independent study notes by Pawan Rathod, converted from handwritten pages into a readable README. Covers ITAM fundamentals, Hardware Asset Management (HAM), Software Asset Management (SAM) and implementation best practices.*

---

## Table of Contents

- [1. What is an Asset and Why ITAM Matters](#1-what-is-an-asset-and-why-itam-matters)
- [2. ITAM Disciplines](#2-itam-disciplines)
- [3. Foundational Data Model](#3-foundational-data-model)
- [4. Asset vs Configuration Item (CI)](#4-asset-vs-configuration-item-ci)
- [5. How Assets Are Created](#5-how-assets-are-created)
- [6. Linking Assets and CIs (IRE)](#6-linking-assets-and-cis-ire)
- [7. Data Normalization](#7-data-normalization)
- [8. Hardware Asset Management (HAM)](#8-hardware-asset-management-ham)
  - [8.1 Cost Optimization and Decision Support](#81-cost-optimization-and-decision-support)
  - [8.2 Hardware Asset Lifecycle](#82-hardware-asset-lifecycle)
  - [8.3 Consumables](#83-consumables)
  - [8.4 Stockrooms and Stock Rules](#84-stockrooms-and-stock-rules)
  - [8.5 Transfer Orders and Purchase Orders](#85-transfer-orders-and-purchase-orders)
  - [8.6 ITSM Asset Management vs HAM](#86-itsm-asset-management-vs-ham)
  - [8.7 Challenges](#87-challenges)
  - [8.8 Product Models and Bundles](#88-product-models-and-bundles)
  - [8.9 Content Service Library](#89-content-service-library)
  - [8.10 Enforce CI Verification and Asset Tracking Strategy](#810-enforce-ci-verification-and-asset-tracking-strategy)
  - [8.11 Asset Retirement](#811-asset-retirement)
  - [8.12 Loaner Assets](#812-loaner-assets)
- [9. Software Asset Management (SAM)](#9-software-asset-management-sam)
  - [9.1 Software Assets and SAM Terminology](#91-software-assets-and-sam-terminology)
  - [9.2 Licenses and Entitlements](#92-licenses-and-entitlements)
  - [9.3 Getting Software Data In](#93-getting-software-data-in)
  - [9.4 Normalization and Reconciliation](#94-normalization-and-reconciliation)
  - [9.5 Removal Candidates and Reclamation](#95-removal-candidates-and-reclamation)
  - [9.6 Downgrade Rights](#96-downgrade-rights)
  - [9.7 Starting a SAM Implementation](#97-starting-a-sam-implementation)
  - [9.8 License Types, Metrics and True-Up](#98-license-types-metrics-and-true-up)
  - [9.9 ServiceNow Subscription Model](#99-servicenow-subscription-model)
  - [9.10 Software Models, Discovery Models and Discovery Maps](#910-software-models-discovery-models-and-discovery-maps)
  - [9.11 Content Data Service](#911-content-data-service)
  - [9.12 Virtual Agent License Allocation](#912-virtual-agent-license-allocation)
  - [9.13 Microsoft SCCM](#913-microsoft-sccm)
  - [9.14 Publisher Packs](#914-publisher-packs)
  - [9.15 Adobe, SaaS and Cloud Integrations](#915-adobe-saas-and-cloud-integrations)
- [10. Best Practices for Implementing Asset Management](#10-best-practices-for-implementing-asset-management)
- [11. Working on SAM: New Implementation vs Support](#11-working-on-sam-new-implementation-vs-support)

---

## 1. What is an Asset and Why ITAM Matters

**Asset:** any tangible or intangible entity that has value or passes value to the organization.

- **Tangible:** something in physical form (for example, a house or gold you own). *Hardware is tangible.*
- **Intangible:** *software is intangible.* Contractual or financial details are also examples of intangible assets.

**Purpose of asset management:** to manage assets effectively and optimize cost.

Organizations want to optimize cost. Some reduce head count; others manage their assets and IT inventory effectively and cut unnecessary overheads. This is one of the strategies used to save a lot of money over the financial year.

An asset is tracked throughout its life for **financial, contractual and lifecycle data**.

**A component should be considered an asset if you want to:**

1. Manage its procurement, receiving, maintenance or retirement.
2. Track its inventory.
3. Manage its associated license, warranty, lease or maintenance contracts.
4. Track its monetary value or incurred costs (for example purchase cost and depreciation).
5. Document its service status, end of life, or decommission.
6. Know who is using it and how often it is being used.

Assets are created or acquired, used to support business activities, managed financially and operationally, and then retired or expired at the end of their useful life. They include relevant data such as asset tag, model category, model and depreciation.

By digitizing ITAM processes you eliminate reliance on manual spreadsheets and paper records, which risk inaccuracies and inefficiency. Software solutions provide real-time updates and notifications so you can track movement and maintenance needs of assets with ease.

**Why asset management?**

- Improved visibility and tracking.
- Reduced cost and increased savings.
- Effective inventory management.
- Supports audit and regulatory compliance of the organization.
- Eliminates security gaps.
- Reduces team inefficiency while boosting overall productivity.

> "If you have to measure something, you have to manage it. If you are not managing it, you can never measure it."

[Back to top](#table-of-contents)

---

## 2. ITAM Disciplines

| # | Discipline | What it covers |
|---|---|---|
| 1 | **Inventory management** | Access stock information; create and manage stock information; edit stock rules, stockrooms and stockroom types. |
| 2 | **Financial management** | Manage fixed assets, depreciation and distribution costs; work with expense lines and rate cards; handle expense allocations. |
| 3 | **Contract management** | Create, edit, renew/extend, and deactivate/delete contracts. |
| 4 | **Model management** | Manage models and their critical processes that ensure good data for procurement and finance; manage model categories. |
| 5 | **Vendor management** | Manage vendor cost, vendor SLAs and related contracts. |
| 6 | **Hardware asset management** | All of the items above, plus CIs; manages hardware and consumable assets. |
| 7 | **Software asset management** | All of the items above; manages all software assets and licenses. |
| 8 | **Data asset management** | Databases, domain names, image libraries, fonts. |
| 9 | **Configuration management** | Not specifically an asset discipline, but works closely with asset management. |

[Back to top](#table-of-contents)

---

## 3. Foundational Data Model

These are a few tables used across the entire ServiceNow platform, whether it is ITSM, ITAM, ITOM, GRC, ERM, HRSD or anything else. They are important not only for ITSM but for every module of ServiceNow, which is why ServiceNow calls this the **foundational data model**.

**Tables:** user, group, location, company, department, knowledge.

[Back to top](#table-of-contents)

---

## 4. Asset vs Configuration Item (CI)

A **Configuration Item (CI)** does not track financial, contractual or warranty details. It holds only the **operational values** of that record or entity. For example, a server has some operational values that change over time, plus specific details that do not change.

| Area | Data nature | Examples |
|---|---|---|
| **Asset management** | Mostly static data | Cost, serial number, asset tag, warranty, contracts |
| **Models** | Common attributes in both | Manufacturer, model |
| **Configuration management** | Dynamic data | Operating system, memory, CPU, network information, file system, installed software |

**Table hierarchy (from the notes' diagram)**

- `cmdb` (Base configuration item) → `cmdb_ci` (Configuration item)
- `alm_asset` (Asset) has these child tables:
  - `alm_hardware`: Hardware
  - `sn_ent_asset`: Enterprise Asset
  - `alm_consumable`: Consumable
  - `alm_facility`: Facility
  - `alm_license`: Software entitlement
  - `alm_information_asset`: Information Asset

**A CMDB CI comprises two parts:**

1. Configuration details.
2. Relationship of this CI to other IT infrastructure CIs.

> "Not every CI can be an asset, but every asset can be a CI."

- **Virtual CIs** (for example VMware virtual machine instances) are never considered assets.
- **Hardware CIs** (computer class, server class, router, network gear) will create an asset. In ServiceNow, a business rule fires to create an asset on insert (Asset from CI); other business rules handle CI → Asset.

[Back to top](#table-of-contents)

---

## 5. How Assets Are Created

No organization creates assets manually at scale. ServiceNow offers multiple data sources, and on top of that you can build **custom integrations** when connecting a third-party tool.

**Potential data sources for creating assets**

- ServiceNow Discovery (runs on IP address)
- ServiceNow Agent Client Collector (ACC)
- Spreadsheets
- Deployment tools (Microsoft SCCM, Intune, Jamf)
- Inventory tools (legacy)
- Directory services (Active Directory, LDAP)
- Human resources systems (Workday, SuccessFactors)
- Enterprise resource planning systems
- Vendor Advance Shipment Notifications (ASN)

**Advance Shipment Notification (ASN)** lets you automate and create asset records while assets are still in transit from the vendor, before physical receipt.

- ServiceNow provides a standard template for importing vendor-shipped asset records, available as part of the Advance Shipment Notification feature.
- Navigation: **Procurement > Orders > Import Shipment Notification**.
- Example: if you buy 100 laptops from Lenovo, the vendor sends an invoice or a template. Even before the items physically arrive, the asset records can already be created from it.

**Integration with purchasing (procurement) systems:** the procurement system holds order history; if integrated with ServiceNow it automatically creates assets and related tasks. Examples: Coupa, SAP Ariba, Oracle Procurement and a few others.

[Back to top](#table-of-contents)

---

## 6. Linking Assets and CIs (IRE)

**How are Asset and CI linked?** A business rule links the asset to a CI. On top of that there are two system properties:

- `glide.asset.create-ci-with-ire`
- `glide.create-alm-asset.async`

**IRE: Identification and Reconciliation Engine.** One of the core components of the CMDB. It is an API provided by ServiceNow that maps values and decides whether to **create a CI or update an existing CI**.

**How fields stay in sync between asset and CI** (these are interlink tables, not core asset management tables):

1. Asset CI field mappings
2. Asset CI Install Status mapping
3. Asset CI Hardware Status mappings

[Back to top](#table-of-contents)

---

## 7. Data Normalization

**Data normalization** is the process of cleaning, standardizing and enriching discovered asset data to fix inconsistent names, missing fields or duplicate values.

**How normalization works**

- **Content service comparison:** ServiceNow compares imported or discovered asset records against official, curated libraries such as the Hardware Asset Management content service and the software content service library.
- **Field updates:** standardizes fields like manufacturer names, product models, versions and device types without changing your original raw data entries.
- **Scheduled jobs:** automated background jobs (such as daily hardware normalization routines) continuously evaluate and update normalization status flags. They can also proactively fix old company references.

**Benefits**

- Clean inventories: messy variations of company names are collapsed to a single standard (for example "Microsoft", "Microsoft Corp", "microsoft inc" become one entry).

[Back to top](#table-of-contents)

---

## 8. Hardware Asset Management (HAM)

A CI may or may not have financial values assigned, but it never has depreciation linked to it. You track a CI when you want to:

- Monitor its technical specifications and operational usage.
- Associate it with an incident, problem or change record.
- Know its relationships to other items.

> An organization may decide to track a component as just an asset, just a CI, or both.

IT services run on IT assets, so assets pose significant challenges and opportunities from a management, cost and risk perspective. To mitigate risk and optimize cost, an organization needs to know: **what hardware assets it has, where they are, and who uses them.**

### 8.1 Cost Optimization and Decision Support

**Cost optimization examples**

- **Optimize contracts:** warranty repairs. If an asset is within warranty, don't pay for an outside repair; ask the vendor to repair it per contract terms and avoid unnecessary upfront cost.
- **Standardize assets:** standardize hardware models to manage fewer kinds of hardware.
- **Employee separation:** collect company assets when employees leave.
- **Avoid penalties:** end-of-lease return or renewal. If you leased assets (for example 10 assets for one year at 10,000 per month) and don't return or renew them by the end date, the vendor may charge a penalty for the extended use.

**Decision support for leaders (CFO, CEO, asset management team)**

- **Asset reporting:** report on missing or lost assets or leverage existing stock. For example, an asset was purchased but may have been stolen, sent for repair and never collected back. Reports also show stock levels, so you know whether to order more based on demand.
- **Asset dashboards:** gauges on assets by manufacturer.
- **Ad-hoc queries:** identify machines for OS upgrade. Example: Microsoft ended support for Windows 10, so IT must identify all machines running it (for example 1000+) and upgrade them to Windows 11. Otherwise they become vulnerable to attack with no vendor support because they are end of life.
- **Data integration:** use information where needed for planning or reconciliation.

### 8.2 Hardware Asset Lifecycle

There are **nine stages** in the IT hardware asset lifecycle, following the asset from planning through sunsetting. Configuration management and IT asset management are closely linked.

| Record type | Stages |
|---|---|
| **IT asset management (Asset record)** | Request → Procure → Receive → Stock → Dispose |
| **Configuration management (CI record)** | Deploy → Monitor → Support → Move, add and manage |

**Walkthrough (example: 10 laptops for new hires)**

1. **Request:** submit a request in ServiceNow for 10 laptops of a given model.
2. **Procure:** the request goes to the procurement team, who handle vendor negotiation, contract negotiation and balance-sheet feasibility, then procure on the company's behalf. Once purchased, the organization owns the asset (laptops, servers, databases, routers, switches, mobile phones).
3. **Receive:** mark the items as received from the vendor.
4. **Stock:** put them in stock. Not all employees join at the same time (one may join a week or a month later), so you manage stock in the meantime.
5. **Deploy:** when stock is assigned to an employee, the asset is deployed and its lifecycle phase moves to configuration management.
6. **Monitor:** watch operational attributes: RAM, whether RAM needs increasing, whether the OS needs upgrading.
7. **Support:** provide support and run maintenance lifecycle activities.
8. **Move, add and manage:**
   - **Move:** when an employee leaves, reclaim the asset and assign it to a new employee.
   - **Add or change:** change the asset's purpose. Example: a Windows server hosting a website that is no longer required is not discarded; it is **repurposed** to host an application or run database services. Use every component of an asset until it is retired.
9. **Dispose:** after 5 or 10 years (per company policy), move it to sunset and dispose of it.

### 8.3 Consumables

**Consumables** are low-cost or low-value items that are generally part of a given asset. Example: a laptop needs a keyboard, mouse, external monitor, headphones, speakers and so on.

Typically consumables are:

- Consumed in general at low prices.
- Used by employees or not.
- Ordered in bulk (not one or two pieces), which gets a discounted price.
- Low value if lost or stolen.

**Non-discoverable:** if you run Discovery or a custom integration to manage consumable inventory, some items are not discoverable and must be managed manually. Once they are in the consumable table, you manage their lifecycle and track them.

**When do we need consumables?** To ensure items are always in stock. Example: you don't want to run out of printer toner or cartridges for a shared printer, so you must track how much exists and where.

**Receiving consumables into a stockroom:** if all of the following fields match, the data merges to create a blended quantity and cost:

- Model category
- Model
- Stockroom / location
- Assigned to
- State
- Sub state

**After a consumable is consumed**, the record remains in the system for reporting. To consume consumable assets they must have **quantity > 0**, **state = In stock** and **substate = Available**.

### 8.4 Stockrooms and Stock Rules

**Stockroom:** used to keep or stock inventory whenever it is not in use, whether hardware assets, consumables or any other assets the company manages.

- You can create stockrooms in ServiceNow and define the location of each.
- All assets in stock are supposed to be assigned to a stockroom.
- **Stock rules** send notifications or automatically transfer assets when a low quantity threshold is detected in a stockroom. They automate how stock levels are tracked and replenished.
- A **manager** should be identified for each stockroom to handle stock validation and transfers in and out.

Stockroom types with specific purposes include:

- Central warehouse
- Warehouse
- Pick up / Drop off (PUDO)
- Forward shipping locations (FSL)
- On site (i.e. local stockroom)

### 8.5 Transfer Orders and Purchase Orders

**Transfer order lines** allow the transfer of multiple assets in one transfer order.

- When you create a transfer order line, a **transfer order line task** is created automatically, based on the model category specified in the asset.
- As you close the task and it moves from one stage to the next, the asset is updated automatically.
- Example: when the line moves from *Ready for fulfilment* to *Prepare for shipment*, the asset's status moves from *Available* to *Reserved*.

**Purchase order:** a binding contract between a buyer and a supplier that authorizes a purchasing action. It contains the description, quantities, prices, applicable discounts, payment terms, delivery dates and other associated terms and conditions with the supplier.

### 8.6 ITSM Asset Management vs HAM

**ITSM Asset Management includes:**

- Asset and model records
- Stockrooms
- Transfer orders
- Purchase orders
- Contracts
- Mobile asset receiving
- Mobile my assets

**Hardware Asset Management** provides additional features on top of ITSM Asset Management:

- Lets you normalize hardware models.
- Gives you the hardware asset workspace.
- Lets you automate the asset lifecycle.
- Asset inventory management features, and many more.

### 8.7 Challenges

- **Driving user adoption:** users must follow each process and fill in all form details. If the procurement team already knows how a ServiceNow instance works it is easy; if they are transitioning from another tool, you must schedule multiple sessions, show step-by-step what comes next and guide them on how to receive purchase orders.
- **Keeping the inventory updated:** creating an asset is not the end. You must keep updating it and managing its lifecycle throughout.
- **Data quality from data sources:** only bring data you are confident is error-free and consistent. Integration can bring a lot of junk data, and you spend lots of time cleaning it. Always clean data at the source (third-party tool) first. If you cannot, filter bad records out in the **transform map** so only clean data reaches the asset table.
- **Foundational data:** effective asset management requires good foundational data. There is a cost to any information tracked, both to obtain and to maintain.

### 8.8 Product Models and Bundles

- A model can belong to one or more model categories. For example, a laptop can be a computer and a server.
- Every hardware item has a **unique model number**: the manufacturer part number for a specific asset configuration the manufacturer provides.

**Bundled model:** a single model composed of individual models (for example a laptop, printer, keyboard and mouse).

- An asset bundle goes through the whole asset lifecycle: building the bundle, allocating it to a user, moving it to maintenance if repairs are needed, and finally retiring it when no longer in use.
- When you **retire** an asset bundle, all assets belonging to it are released (disassociated) and returned to a stockroom.
- The bundle can then be deleted, and the disassociated assets can be disposed of individually.
- **Deleting** an asset bundle moves all assets belonging to the bundle to a stockroom.

### 8.9 Content Service Library

ServiceNow has many customers (for example customer ABC), and each customer may have a different set of models, custom or out of the box, plus different vendors and manufacturers.

**How ServiceNow keeps content up to date:** it has a **content service team**, a small team whose goal is to push updates from the market: new models, their lifecycle, model information, and sometimes procurement data. They gather this from the public internet, reconcile it and make sure it is accurate and up to date.

- The content service library lives in a **separate ServiceNow instance with its own database**, different from the customer's instance.
- On a periodic basis ServiceNow pushes updates to all customers.
- A customer can **opt in or out**: if a customer doesn't want all updates pushed to its instance because it follows its own practices, ServiceNow provides that flexibility.
- ServiceNow may also pull some data from customer instances (an inclusion/opt-in), validate it, check for errors, and then add it to the content library.

**If you opt in, HAM can normalize these 6 tables' records:**

| Table | What it holds |
|---|---|
| Device type | Type of device, such as computer, printer, monitor. |
| Hardware lifecycle definition | Lifecycle phases of a hardware or consumable model and the associated dates. |
| Hardware manufacturer | Name of the hardware manufacturer. |
| Hardware model library | Name of the product and model numbers. |
| Hardware normalization map | Hardware product and hardware product model mappings. |
| Hardware product | Name of the product and type of device, such as Apple Mac or Lenovo printer. |

**Scheduled job name:** `HAM - Apply Latest Content Changes`.

### 8.10 Enforce CI Verification and Asset Tracking Strategy

**Enforce CI verification:** if you activate this option on a specific model category, assets are **not created** for new CIs in that model category until they are verified. There are two ways to verify a CI:

- **Create Asset:** verifies the CI and creates the asset. This deselects the *Requires verification* flag on the CI record.
- **Merge CI:** merges the CI record with another CI that has an associated asset. Useful when an asset and CI were created separately but refer to the same device.

**Asset tracking strategy:** in addition to the Enforce CI verification setting on model categories (which affects all models in that category), you can affect **individual model records**. The *Asset tracking strategy* field overrides the standard "create asset" behavior. Options:

| Option | Behavior |
|---|---|
| **Leave to category** | Behavior of assets and CIs of this model is based on the model category configuration. |
| **Create consumable asset** | Treat this model as a consumable rather than an individually tracked asset. Useful for printers: track expensive ones as individual assets but low-end ones as consumables. |
| **Do not create asset** | Do not create assets for CIs of this model. Useful for models you know your company does not purchase. |

### 8.11 Asset Retirement

An organization retires assets when they are no longer required or can no longer comply with current requirements.

**It is not just changing the state.** Unlike an incident (where changing state to closed completes it), a retired hardware asset supports multiple software and application components. For example, a laptop may have tools consuming licenses (Office 365: Word, PowerPoint and so on). Marking the laptop as retired does **not** remove the software or licenses. ServiceNow ITAM suggests setting the state to retired, but that doesn't account for the different tasks needed to make sure the asset is fully retired.

**Before or while retiring, make sure to:**

1. **Remove all software licenses** from the asset.
2. **Remove company-specific sensitive data** (downloaded files, personal information shared with HR or other teams). The retired asset must contain no company or personal data.
3. **Check whether it can be reused or repurposed.** Example: an email server being sunset that cost a lot of money is repurposed instead of simply retired.
4. **Follow the destruction procedure:** does the company destroy the asset itself or use an external company that specializes in asset destruction? This differs from company to company.

**What to do with the asset (scope varies by organization):**

- **Donate** it to charity if not usable. The charity benefits, and it supports the company's CSR (corporate social responsibility) activity.
- **Return to the original vendor** for **buyback value**. Example: bought MacBooks from Apple on contract; after 5 years Apple takes old laptops and gives a discount on new ones based on the old asset's value.
- **Destroy** the asset and all its data if the organization is under strict compliance and regulation (for example government-related entities).
- **Preserve it as is** if there are court cases involving the organization. Keep the asset until the final verdict.

**Asset retirement workflow**

| Step | Type | Action |
|---|---|---|
| 1 | Create task | Back up / remove user files |
| 2 | Create task | Wipe drive |
| 3 | Automate | Remove device entitlement |
| 4 | Automate | Remove software installations |
| 5 | Leased? | **Yes:** create task, return to vendor. **No:** create task, send to disposal vendor. |
| 6 | Automate | Update asset record |

(The flow is named **Hardware Asset Disposal Flow**.)

**Disposal reports** help manage the process:

- Assets retired in the past month.
- Status of assets as they progress through retirement.
- A list of assets scheduled for retirement within the next month, to plan resources.
- If you work with a disposal vendor, you may receive a **disposal certificate** for each disposed asset. Attach the digital certificate to the retired asset record as proof of adherence to environmental, regulatory and legal requirements.

### 8.12 Loaner Assets

**Loaner / temporary assets** are given on top of your primary asset.

- Example: you have a Windows laptop but work in an iOS development team and must test an application. You could create Mac OS VMs, but generally you need a MacBook for testing. You request a temporary asset; once approved, the asset management team hands it over on a temporary basis, and you return it after testing.
- Within the stockroom, loaner assets are marked separately as those that can be loaned to users.
- Loaner assets are **not counted** in the stockroom count when stocktakes are processed.

[Back to top](#table-of-contents)

---

## 9. Software Asset Management (SAM)

### 9.1 Software Assets and SAM Terminology

A **software asset** is not a hardware asset. It falls under the intangible form: it has no physical existence, but it is a digital product of a company. Example: Microsoft Office 365, Zoom, web browsers. Whether licensed or not, they are software installed on your system.

For **enterprise software asset management**, only software that has **financial implications** (costs the enterprise) is considered an asset. It is a main focus area because organizations want to optimize license cost and ensure licenses are neither over-purchased nor under-purchased.

**Terminology and UI**

- Before the Utah (major) release, ServiceNow had a Software Asset Management **classic user interface**. ServiceNow is moving every product (SAM, HAM, ITAM, GRC) to **workspaces** for a unified experience across the platform, and has reduced support for the classic interface, so customers are being asked to migrate to the workspace.
- **SAM dashboard** (similar to HAM): shows license summary, compliance overview, normalization and content normalization.
- Software models created for all installed software are used to normalize software installations.

### 9.2 Licenses and Entitlements

- **License** (also software license or product license): in formal terms, the **right to use** a product that you purchased or acquired. It may be for lifetime or a certain duration, depending on the vendor's licensing terms and conditions.
- In SAM we use **license entitlements**, tied to a user or device.
- **Entitlements** are the licenses you have purchased. Example: if you purchased 40 licenses for Office 365, you create an entitlement record stating you purchased 40 licenses.

### 9.3 Getting Software Data In

Similar to HAM, you bring software data in via different data sources. But software data points are huge: 1000 devices don't map to 1000 software items, because each device supports multiple software at the same time.

- ServiceNow **recommends not creating custom integrations**. The best way to get software license details is to enable **enterprise-wide ServiceNow Discovery** or **Microsoft SCCM**.
- ServiceNow offers its own **Discovery**, and the **Microsoft Service Graph Connector for SCCM** (a plugin you install, which pulls data from SCCM). This pre-populates the software installation table with discovered information, so you don't have to worry about bringing in and then reconciling data first.
- Enterprises often run both in parallel, because Discovery may not discover **end-user devices**. End-user devices are assets given to employees; unless the employee connects to the organization network or subnet, ServiceNow Discovery cannot discover them.
- **Microsoft SCCM** is agent-based: it queries each end-user asset and stores the data in a database. You integrate ServiceNow with SCCM rather than directly with end-user devices.

### 9.4 Normalization and Reconciliation

- **Normalization:** standardize publisher, discovered product and version against ServiceNow's repository of normalized equivalents.
- **Reconciliation:** calculate the **compliance status** of software products based on entitlements.
  - Example: you purchased 40 Office 365 licenses and created an entitlement. Reconciliation brings in installed software data (from Discovery or SCCM) and validates whether all 40 entitlements are used or some are left to allocate to other users.
- A scheduled job drives software license reconciliation.

### 9.5 Removal Candidates and Reclamation

**Removal candidates:** software not used for a certain duration. ServiceNow recommends reclaiming those software rights to optimize license cost.

- Example: Office 365 was assigned to a user who hasn't used it for a year, but you still pay Microsoft every year. SAM flags that this user hasn't used the software for many days, so you can reclaim it and optimize cost.

**Reclamation flow**

- A flow triggers when you reclaim software; it is part of SAM.
- When you **close complete** a reclamation task, the license is added back to the available license pool and the **potential savings** are added to your total ownership cost.
- **Closed / Skipped:** the task is generated, but you manually override it because the software must remain installed on the system or device. If you close (skip) it, the license is **not** added back to the bucket.

### 9.6 Downgrade Rights

You acquired a license for a higher version, but it still supports older versions. **Downgrade rights** let you use the earlier version under the license of the latest version.

- Example: you purchased Windows 11 licenses, but some employees prefer to stay on Windows 10. You don't need to buy Windows 10 licenses again; they are covered under downgrade rights of the Windows 11 license.
- Similarly, an Android phone shipped with Android 14 or 15 can be upgraded for free to Android 16; the rights are covered without extra charges.

### 9.7 Starting a SAM Implementation

To start implementing SAM (for example for Office 365):

- You need **Discovery, SCCM, another connector plugin or a custom integration** that brings software installation data. Without software installation data it makes no sense to start SAM maturity or implementation. First get the data; this is the **foundational data** for SAM.
- You can **create entitlements from Excel** or a standard form. Important fields: **publisher part number** and **software model** (an asset is not required). Also know the **license status** (perpetual, subscription-based, upgrade or maintenance) and the **license metric type**.

### 9.8 License Types, Metrics and True-Up

**True-up cost:** the cost an organization incurs to **remain compliant**.

- If a product is compliant, true-up cost is zero. If not, you see some cost.
- It is license required multiplied by the cost of each license.
- When you hear that "true-up cost increased" or "reduced", it reflects how many licenses are under-licensed or required to remain compliant.

Each publisher may have a different type of licensing:

| License type | Description |
|---|---|
| **Perpetual** | Purchase once and use for lifetime. Only a one-time cost, usable across the entire organization. |
| **Subscription based** | Fixed duration (monthly, 6 months, 12 months), like a Netflix subscription. Time-limited capability; you must purchase a license again after expiry. Some publishers are fully subscription based. |
| **Upgrade** | A license type for upgrading. Example: you own Windows 10 licenses and want Windows 11. Creating an entitlement with upgrade means you don't need to buy an additional license; it is covered under the already purchased license. |
| **Perpetual plus maintenance** | One-time cost for the license plus recurring maintenance. Example: software ABC costs $100 one time, and support costs an additional maintenance fee of, say, $20. Maintenance is recurring but lower than the perpetual license cost. |
| **Maintenance** | Requires a base license (perpetual or subscription) to exist before you can create maintenance. |

**License metric:** depends on the publisher. Every publisher has its own metric type, which makes tracking hard. Examples:

| Metric | Meaning |
|---|---|
| **Employee** | You purchase for employees; each employee can use the software under his or her license. |
| **Named user plus** (Oracle) | Unique username throughout the organization; the license is shared only with that user. |
| **Per processor** | Based on servers or hosts. With multiple multiprocessor hosts, each processor counts toward license count. |

### 9.9 ServiceNow Subscription Model

**Subscription unit ratio:** determines, for one license purchased from ServiceNow for ITAM, how many CIs or assets you can maintain or manage.

- Example: for **end-user computers, 4:1** means you can manage four computer-class assets and it accounts for only **one license**.
- For **servers (SAM Enterprise), 1:1**: one license manages one server.
- The first ratio (SAM Enterprise, end-user computers) tells how many CIs or assets one license covers; the second (SAM Enterprise, servers) tells how many licenses are required.
- This applies to other categories as well, not only computers and servers.

Organizations want every publisher in their environment to be compliant, but rarely at once: they may start with Microsoft, then IBM, and so on, gradually.

### 9.10 Software Models, Discovery Models and Discovery Maps

- **Software model:** a well-recognized model used across the organization. You get software model updates from the ServiceNow content service library. It has a standard name (for example Adobe Acrobat).
- **Discovery model:** whatever ServiceNow Discovery (or another integration) brings in for the same software, which may have different variations.
- **Discovery maps:** map the discovery model to the software model. They come from the content service library and hold the publisher, product and edition. Once a software model is tied, it has a mapping of discovery model to software model.

Rules to remember:

- A software model is only created if **no match** is found.
- It is created across versions and for a specific edition if available, but **never against a specific version**.
- Discovery maps are associated to software models only.
- If no corresponding discovery map exists in the content service library, a software model can be created without a discovery map.
- Properties let you **automatically create software models** for all "licensable" products, or for "not licensable" products. When Discovery runs and finds software with no appropriate software model, it creates one if the property is **true**; if **false**, it does not.

### 9.11 Content Data Service

The **Content Data Service Library (CDS)** transfers data between your instance and ServiceNow. The SAM content service provides **weekly** content updates you can use to normalize your data.

**SAM content types**

- Categories
- Discovery maps
- Normalization suggestions
- Product classification
- Product license exception rules
- Product names
- Product types
- Publishers
- Suite definitions
- Software model lifecycles
- Software model suggestions
- Custom software product suggestions
- Common Platform Enumeration (CPE) normalization rule

**HAM content types**

- Hardware models
- Consumable models
- Hardware lifecycle
- Consumable lifecycle
- Custom product model
- Hardware normalization map
- Hardware product
- Device type

### 9.12 Virtual Agent License Allocation

If **Virtual Agent** (chatbot) is installed in your instance, you can automate the license allocation process. For example, an employee who wants software installed on their system can request it via Virtual Agent and say which software they need; the agent applies the allocation logic.

- Rights are allocated to the specified user or device in **ascending chronological order**.
- You can have **multiple entitlements** for the same software; any rights left on an **older entitlement** are allocated first, then it starts allocating from the newer one.
- If software supports both **user and device allocation**, Virtual Agent gives **priority to user allocation** first; once user allocation is exhausted, it starts the device allocation workflow.

### 9.13 Microsoft SCCM

**SCCM** is a product offered by Microsoft to manage devices, mostly Windows devices, though it can manage others too. It is now known as **Microsoft Endpoint Configuration Manager** (formerly System Center Configuration Manager). It is widely used for deploying, configuring and securing devices, and is a platform where you can control all devices within the organization. It is generally used to track **license usage or consumption for on-premise applications** (for example Excel and Word).

- You integrate SCCM with ServiceNow via the **Service Graph Connector**.
- For an SCCM integration there is usually a specific team that configures the SCCM end and shares details such as the database endpoint and port; you then configure these in the ServiceNow instance. Without them you cannot integrate.
- The SAM dashboard handles license calculation and metrics tracking.

### 9.14 Publisher Packs

In the software industry, publishers are grouped into **tiers**:

- **Tier one:** companies whose products are widely used across industries (IT, manufacturing, medical, education).
- **Tier two:** lower market penetration but still comparable.
- **Tier three, four:** and so on.

**Publisher packs** are ServiceNow add-on plugins provided for these publishers. They are **not included by default** with SAM or SAM Pro; you must install them additionally for each publisher.

- Rarely do you see all publishers implemented and reconciled at once, unless you've reached a maturity level where all details flow into ServiceNow without bundles. Typically you start with Microsoft; once it is mature, move to IBM, Citrix or Adobe.
- Calculation is based on the **agreement**, which the publishers provide and the procurement team signs. The procurement team hands these details to you.

**About 80% of software spend is likely with the top few publishers.** Publisher pack support includes:

| Publisher | Metrics supported |
|---|---|
| **Microsoft** | User CAL, device CAL, per core, per device, per processor, virtual use, per user, Visual Studio, subscriptions |
| **Oracle** | Named user plus, per processor, partitioning, management packs, Solaris named, unlimited licensing |
| **IBM** | Virtual processor core, processor value unit, resource value unit, sub capacity, full capacity, per device, per user, per processor, per named user; Red Hat hubs, sockets; ASP partner support |
| **VMware** | Per app instance, per processor, per named user, per host, per device |
| **Citrix** | Concurrent users, per device, per processor, per user, per user/device |
| **SAP** | Named user, engines, indirect access, maintenance, contracts |
| **Adobe** | Per device, per user, user subscriptions, OAuth |

**Common and custom metrics**

- About **20% of software spend** is with long-tail publishers, typically licensed by common metrics in the content library: per core, per device, per named device, per named user, per processor, per user.
- Other coverage noted: SaaS discovery (around 10K apps, 1500 categories, ~80 direct-to-vendor connections, any SaaS app via Azure AD and Okta SSO) and **concurrent** metrics (25+ engineering products via OpenLM; floating, network and token licenses).
- **Custom metrics** (examples: revenue metrics, head count, projects, subcontracts, traffic metrics) are for complex cases such as revenue share, software compliance or in-house apps.

**Why these publishers matter:** most organizations use their products. Microsoft is common across industries. Oracle appears with databases and Oracle Cloud. IBM covers ESX, virtual processors, virtual machines and Red Hat. VMware is for virtual machines and vCenter. Citrix is for VDI. SAP provides engines for on-premise.

### 9.15 Adobe, SaaS and Cloud Integrations

- **Adobe** mainly offers SaaS-based products, so you will have user subscriptions, per device and per user metrics.
- Procurement hands over the agreement (for example Adobe licenses for 100 users under an Enterprise Term License Agreement, with a metric of per device, per user or user subscription). Based on that, **you create the entitlement**, then ensure sufficient software installation and usage data flows in from the device or user, and finally **reconcile and generate SAM dashboard metrics** showing compliant or not.
- Procurement handles negotiation and renewal deals with vendors; you (as a SAM developer) don't negotiate with vendors.
- **Adobe cloud integration:** since Adobe SaaS data lives in the cloud, you can't pull it from on-prem devices. Integrate (via a spoke) with the cloud console or portal of Adobe to bring in subscription data for all users and usage metrics. The plugin also adds reporting capabilities and Adobe cloud usage without installations.
- The plugin comes with script includes and backend code. It is **generally recommended not to modify** them, as they follow agreements and policies with the tier-one publishers. If Adobe changes its licensing terms, ServiceNow updates the pack.

**SCCM vs SaaS**

- **SCCM:** tracks license usage or consumption for **on-premise** applications.
- **SaaS** applications are accessed via web browser and live in the cloud, so SCCM cannot track their license consumption. You must **integrate directly with the SaaS provider** (for example Adobe Cloud), using a custom API or whatever ServiceNow offers out of the box.

[Back to top](#table-of-contents)

---

## 10. Best Practices for Implementing Asset Management

| # | Practice | Key points |
|---|---|---|
| 1 | **Define and document the process** | Undocumented process drives inconsistency and lack of repeatability. Work closely with the business analyst, who documents the end goal, current maturity level, next steps this quarter and next quarter, a roadmap and a time frame. Your work is visible across the organization (CFO, CIO), because they consume the dashboard metrics: potential savings, current license usage, cost that can be optimized. |
| 2 | **Create a baseline** | If the organization already tracks licenses in an Excel sheet or another tool, start from there instead of from scratch. Starting from scratch can take six months to a year just to reach that baseline. Ask the client and procurement team what they currently track (for example, are Adobe publisher licenses tracked anywhere?). If a baseline exists, import it into ServiceNow; if not, start from scratch. Mindset: how can I fast-track development? |
| 3 | **Identify opportunities** | As an ITAM developer, your role is not only to implement but to find where SAM or HAM can improve processes and the organization's financial decision capability. Leaders must be able to trust the data. Dashboards and reports must be built on accurate foundational data. You may not have 100% accuracy at first, but as you mature the data should be trustworthy enough for informed decisions. |
| 4 | **Phased approach** | Don't try to do everything at once; it creates chaos. Plan to implement over time (it may take 6 months or a year). Complete one phase, then move to the next. Different developers can handle different parts if the company allows. |
| 5 | **Prioritize** | Determine the most important and beneficial projects first. Not all publishers have equal priority (for example, prioritize Microsoft if spend is highest). Talk to business owners, product owners and the procurement team. Prioritization is something you can *influence* but not *decide*; it comes from business leaders. |
| 6 | **Involve business units** | Asset management is not meaningful in isolation; it is most valuable when coordinated with other processes and groups. Business owners validate each part of the implementation and guide you on direction, value, metrics and KPIs. |
| 7 | **Communicate value and ensure future support** | If the organization sees no value, it will pause, halt or cancel the project. Communicating the value you bring helps you receive complete support from the client for future activities. |
| 8 | **Define a continual service improvement (CSI) plan** | Determine where you are, where you want to be, create a plan to get there, and a way to stay there. Define an SOP for CSI once the process is mature. This applies to CMDB and asset data as well: ServiceNow adds and deprecates functionality over time, so keep pace and decide whether to stay or move ahead. If it is already 80% implemented, make sure continuous operations can use it without impact on the organization. |

[Back to top](#table-of-contents)

---

## 11. Working on SAM: New Implementation vs Support

**If you start a project from scratch** (a project just beginning to implement asset management), you get more opportunities to learn:

- How to create entitlements.
- How to create reclamation rules.
- How to create and optimize licenses.
- The different license metrics per publisher.
- How the procurement team brings in data.
- How to integrate with the procurement tool.

**If you join as a support resource** to keep things running, you must:

- Monitor SAM licenses and check whether the organization is using unlicensed or over-licensed software.
- Follow up with teams and make them aware of current license counts as you try to optimize.
- Understand the end-to-end flow behind what the current dashboards show.
- Investigate when issues appear, for example Adobe licenses not being calculated correctly.

**Troubleshooting checklist**

1. Check the existing configuration.
2. Verify that data flowing in from the Adobe integration, SCCM or any other third-party tool is accurate and correct.
3. Confirm reconciliation is working as expected.
4. Confirm entitlement allocations are up to date.

[Back to top](#table-of-contents)
