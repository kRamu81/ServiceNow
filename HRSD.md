# ServiceNow HRSD (HR Service Delivery) Notes

*Topic-wise study notes covering plugins, roles, HR profile, HR cases, Centers of Excellence, service configuration, lifecycle events, templates, catalog, e-signature, transfer and bulk cases, Employee Center, reporting, email and integrations.*

---

## Table of Contents

- [1. Plugins Required for HRSD](#1-plugins-required-for-hrsd)
- [2. Required Roles in HRSD](#2-required-roles-in-hrsd)
  - [What is a Role?](#what-is-a-role)
  - [HR Core Roles](#hr-core-roles)
  - [HR Client Roles](#hr-client-roles)
  - [Use of Groups in HRSD](#use-of-groups-in-hrsd)
  - [Finding and Creating Roles](#finding-and-creating-roles)
  - [Ways to Create HR Cases](#ways-to-create-hr-cases)
- [3. HR Profile and HR Profile vs User Profile](#3-hr-profile-and-hr-profile-vs-user-profile)
- [4. HR Case Creation](#4-hr-case-creation)
- [5. Center of Excellence (COE)](#5-center-of-excellence-coe)
- [6. HR Administration (Case and Service Configuration)](#6-hr-administration-case-and-service-configuration)
- [7. HR Case Life Cycle and Events](#7-hr-case-life-cycle-and-events)
- [8. HR Service Configuration, Service Activities, Approvals and Tasks](#8-hr-service-configuration-service-activities-approvals-and-tasks)
- [9. HR Criteria and HR Templates](#9-hr-criteria-and-hr-templates)
- [10. HR Catalog, Record Producers and Service Activities](#10-hr-catalog-record-producers-and-service-activities)
- [11. E-Signature Upload](#11-e-signature-upload)
- [12. Transfer Case and Bulk Case Creation](#12-transfer-case-and-bulk-case-creation)
- [13. Employee Center Pro and ESC Ticket Page Configuration](#13-employee-center-pro-and-esc-ticket-page-configuration)
- [14. HR Reports and Dashboards](#14-hr-reports-and-dashboards)
- [15. Email Configuration (Inbound and Outbound)](#15-email-configuration-inbound-and-outbound)
- [16. HR Integration](#16-hr-integration)

---

## 1. Plugins Required for HRSD

| # | Plugin |
|---|---|
| 1 | `com.sn_hr_core` |
| 2 | `com.sn_hr_sp` |
| 3 | `com.sn_hr_lifecycle_events` |
| 4 | `com.sn_hr_employee_relations` |
| 5 | `com.sn_hr_payroll` |

[Back to top](#table-of-contents)

---

## 2. Required Roles in HRSD

Topics: what a role is, types of roles in HRSD, how to find and create roles, types of HR cases, case creation procedure.

### What is a Role?

A role in ServiceNow defines the permissions a user or group has to access and perform actions on the platform. It ensures secure access by granting specific rights such as read, write or execute to applications and records.

**HRSD role:** in HR Service Delivery, roles define user permissions and responsibilities within the HR domain, enabling efficient management of employee requests and HR processes. Roles are categorized in two ways: **HR Core roles** and **HR Client roles**.

### HR Core Roles

| Role | Name | Purpose |
|---|---|---|
| **HR Admin** | `sn_hr_core.admin` | Manages the HR module, configurations and processes. Can access and configure case and knowledge management. |
| **HR Agent** | `sn_hr_core.basic` | Access for basic HR agents' daily work. |
| **HR Knowledge Contributor** | `sn_hr_core.kb_writer` | Creates and updates HR knowledge articles. |
| **HR Manager** | `sn_hr_core.manager` | Oversees HR operations and performance metrics. |
| **Employee** | `sn_hr_sp.esc_admin` | Employee Service Center admin. |
| **HR Case Writer** | `sn_hr_core.case_writer` | Access to write all HR cases. |

### HR Client Roles

Roles assigned to employees. Various categories of employees exist in an organization, and roles are added to employees based on those categories.

- Client roles containing **"hrsm"** in the name allow access to the **platform**.
- Client roles containing **"hrsp"** in the name allow access to the **Employee Service Center / Service Portal**.

**Department roles (platform)**

| Name | Description |
|---|---|
| `sn_hr_core.admin` | Access to all HR information, plus admin privileges. |
| `sn_hr_core.basic` | Access to basic HR agent daily work. |
| `sn_hr_core.manager` | Access to all HR cases, profiles and secure information. |
| `sn_hr_integrations.admin` | HR Integrations admin; can access the Integrations module and perform related operations. |

**Client roles (platform)**

| Name | Description |
|---|---|
| `sn_hr_core.hrsm_alumni` | Employee who has left the company. |
| `sn_hr_core.hrsm_contingent` | Employee who works temporarily. |
| `sn_hr_core.hrsm_contractor` | Employee who works as a contractor. |
| `sn_hr_core.hrsm_employee` | Full-time employee. |
| `sn_hr_core.hrsm_new_hire` | Newly hired employee. |

**Client roles (portal)**

| Name | Description |
|---|---|
| `sn_hr_sp.hrsp_alumni` | Employee who is alumni. |
| `sn_hr_sp.hrsp_contingent` | Employee who works temporarily. |
| `sn_hr_sp.hrsp_contractor` | Employee who works as a contractor. |
| `sn_hr_sp.hrsp_employee` | Full-time employee. |
| `sn_hr_sp.hrsp_new_hire` | Newly hired employee. |

*Department roles are for HR agents, managers and administrators who manage HR services and cases. Client roles are for employees who submit requests or access HR resources.*

### Use of Groups in HRSD

Groups organize HR teams, assign work efficiently and manage access to HR services.

- **Work assignment:** cases and tasks can be assigned to a group instead of an individual (for example, an "HR Benefits Team" handles all benefits cases).
- **Escalation and workflows:** cases can be routed to different HR groups by category (payroll, benefits, IT equipment requests).
- **Access control (via roles):** members inherit the roles assigned to the group.
- **Collaboration:** HR teams work together within groups to resolve cases.

### Finding and Creating Roles

1. Open the Application Navigator.
2. Navigate to **System Security > Roles** to view, create or edit roles.

### Ways to Create HR Cases

- Instance (manually)
- Employee Self-Service Portal
- HR Agent Workspace
- Email integration
- HR Chat (Virtual Agent)
- Bulk case creation
- Workflows or automation
- API integration

[Back to top](#table-of-contents)

---

## 3. HR Profile and HR Profile vs User Profile

**User profile:** the user record stored in the `sys_user` table. It contains:

1. Personal information: name, email, phone number, location.
2. Roles and permissions: roles assigned to determine access levels.
3. Authentication details: username, password, multi-factor authentication settings.
4. Group memberships: groups the user belongs to, for role-based access control and approvals.

It is foundational for access management, task assignments, notifications and personalized experiences. *(In your PDI, check navigation with `sys_user.LIST`.)*

**HR profile:** stores confidential employee data (name, personal contact information, email addresses, employment history, contacts) in the scoped version of HR. It lets the organization track employee information throughout and beyond employment. HR Profile records are associated with user records but, unlike user records, are stored confidentially and are **not publicly viewable**. Fields editable by employees are defined in HR Properties under *HR profile fields editable by users*. *(In your PDI, check `sn_hr_core_profile.LIST`.)*

**Where the HR profile is used**

1. **HR case management:** profiles are linked to cases to provide employee-specific data.
2. **Employee Center:** personalizes information, notifications and service recommendations.
3. **Lifecycle events:** onboarding, offboarding and other events use employee data to automate and assign tasks.
4. **HR dashboards and reporting:** data for analytics and tracking trends and service performance.
5. **Access control:** restricts access to sensitive employee information in line with HR roles.

**HR profile vs user profile**

| Aspect | HR Profile | User Profile |
|---|---|---|
| Purpose | Employee-specific HR data for HR processes | User data for platform access and authentication |
| Table name | `sn_hr_core_profile` | `sys_user` |
| Data included | HR-specific and sensitive details: job title, department, location, manager | General user details: username, email, roles, groups |
| Usage | HRSD case management, lifecycle events, reporting | System authentication, access control, task assignments |
| Access control | Restricted to HR-specific roles for privacy and compliance | Generally accessible by admin roles; varies by assigned permissions |

[Back to top](#table-of-contents)

---

## 4. HR Case Creation

Topics: what an HR case is, who can create it, ways to create it, required roles, creating cases in platform/portal/HR agent workspace.

**What is an HR case?** A record created to manage and track employee-related issues, requests or inquiries, so HR can streamline processes and resolve them efficiently. Cases can cover:

- Employee grievances
- Disciplinary actions
- Requests for accommodations
- Payroll inquiries
- Benefits questions
- Performance management issues

**Ways to create an HR case**

- Employee Self-Service (Employee Center)
- HR Agent Workspace
- HR Service Catalog
- Emails (Inbound Email Actions)
- Manual creation (backend)
- Virtual Agent (chatbot)
- Integration with other systems
- HR case from HR profile

**Required roles:** see [HR Core Roles and HR Client Roles](#2-required-roles-in-hrsd) above (department roles for HR staff, client roles for employees).

[Back to top](#table-of-contents)

---

## 5. Center of Excellence (COE)

Topics: what a COE is, COE data model and overview, roles to create a COE, COE security configuration.

**What is a COE?** A Center of Excellence is a table structure that helps organize HR data, services and processes by functional discipline, using the HR COE data model. COEs help improve employee experience and simplify processes.

> A COE is the **extension of the core case table** (`sn_hr_core_case`).

Each COE represents a functional area within HR, such as Employee Relations, Payroll, Benefits, Onboarding or Offboarding, and each is tied to a specific table, so you can organize and manage HR services efficiently.

**COE data model and overview:** each HR COE extends the HR Case `sn_hr_core_case` table and is organized around a discipline such as employee relations, lifecycle events, payroll, talent management, workforce administration, total rewards, HRIT operations, benefits, compensation, corporate communications and global mobility.

> **Note:** the benefits, compensation, corporate communications and global mobility COEs are **disabled by default**.

**Required role:** Admin.

**Create a new Topic Category, Topic Detail and HR Service**

```
sn_hr_core_topic_category_list
        ↓
Click any Topic category (sn_hr_core_topic_category_list)
        ↓
Click any new Topic (sn_hr_core_topic_detail)
        ↓
Create any new HR Service (sn_hr_core_service)
```

**COE security configuration:** lets you define group restrictions for specific HR cases and COEs, restricting access to HR data based on conditions and user groups, so you control which HR professionals can view or modify certain cases.

- Create COE security policies that define which groups have read or write access to specific HR cases within a COE.
- Example: restrict HR benefits cases to the HR Benefits team only, so other HR professionals cannot view or modify them.

[Back to top](#table-of-contents)

---

## 6. HR Administration (Case and Service Configuration)

Topics: what HR administration is, case creation configuration, SLA definitions, HR criteria, COE configuration (already covered).

**HR administration** refers to the tools and processes that manage and streamline HR functions within an organization. The following are important configurations used in day-to-day work.

**Case creation configuration:** lets administrators configure rules for creating HR cases based on specific conditions.

- **Page size:** number of records displayed while searching for a user during case creation.
- **Minimum input length:** number of characters required while searching for a user during case creation.

**SLA definitions:** the **Service Level Definition** module lets administrators define and manage SLAs and OLAs specific to HR services, similar to SLA definitions in ITSM. They set expectations for response and resolution times for HR.

**HR criteria:** a set of conditions used to define and segment groups of employees, so the system can determine which employees are eligible for specific HR services, tasks or content.

Example use cases:

1. **Onboarding services:** employees in the "New Hire" group (defined by HR criteria) receive onboarding tasks tailored to their location and job role.
2. **Knowledge base access:** only employees in a specific region or department can view certain HR articles, such as regional holiday policies.
3. **Lifecycle event eligibility:** contractors and full-time employees can have different offboarding workflows based on HR criteria.

[Back to top](#table-of-contents)

---

## 7. HR Case Life Cycle and Events

Topics: what an HR lifecycle event is, documenting the process, configuring a lifecycle event, configuring an HR service for it.

**What is it?** A framework designed to automate and manage complex HR processes that span multiple tasks, departments and systems. Everyone must agree on the flow and their areas of responsibility. As you map out an event, consider **who owns the activity, what the activity type is, and whether there is a targeted audience.**

**Documenting a lifecycle event process**

1. **What is the lifecycle event type?** Examples: employee onboarding, offboarding, parental leave.
2. **What are the stages?** Stages are represented as **activity sets**, containers for the associated activities. Each activity set is triggered based on specified conditions.
3. **What are the activities in each set?** For each activity, decide the owning group, activity type and audience.
   - **Who owns the activity?** Events span multiple departments; each activity must be owned by the responsible group (HR, IT, Facilities).
   - **What is the activity type?** Approvals; tasks for employees; activities for fulfillers (order guides, catalog items, HR services, HR tasks, incidents and more); notifications.
   - **Is there a targeted audience?** Not every activity is required for every employee; use **audience records** to target the right user types.

**Example: new hire onboarding.** Included as demo data with the **Lifecycle Events for Enterprise** plugin (`com.sn_hr_lifecycle_ent`). It is organized into activity sets such as **pre-hire, pre-boarding and day one**. Each set has activities, such as a task for the employee to enroll in medical benefits, or a request for an IT fulfiller to set up a new hire's phone and voicemail.

*Practice: create a real-time example in your instance.*

[Back to top](#table-of-contents)

---

## 8. HR Service Configuration, Service Activities, Approvals and Tasks

HR services are the requests and assistance an HR organization provides to employees. You configure them to define the request-to-fulfillment process for that case type.

- **HR case templates:** automatically populate fields on the HR case form when a service request is made.
- **Employee self-service:** configure a **record producer** to make an existing HR service available as an HR catalog item in the HR service catalog.

**Configure an HR service**

- **Role required:** `sn_hr_core.admin`
- **Navigation:** All > HR Administration > HR Services > HR Service Configuration
- Create a new HR service and fill in the fields.

### HR Service Activities (Process Automation Engine)

Service Activities control **what happens after the HR case is created.**

**Navigation:** HR Service Management → Administration → Service Activities

| Type | Purpose |
|---|---|
| Approval | Manager / HR approval |
| Task | Create HR tasks |
| Flow | Trigger Flow Designer |
| Manual | Informational step |

### Create an Approval Service Activity

**Navigation:** HR Service Management → Administration → Service Activities → New

| Field | Value |
|---|---|
| Name | Manager Approval |
| Type | Approval |
| HR Service | Request Experience Letter |
| Approval Type | Manager |
| Approver | Case → Opened for → Manager |
| Condition | Optional (example: request type = external) |
| Wait for Approval | ✔ |

**Result:** the HR case waits until the approval is Approved or Rejected.

### Create an HR Task Service Activity

| Field | Value |
|---|---|
| Name | Generate Experience Letter |
| Type | Task |
| HR Service | Request Experience Letter |
| Assignment Group | HR Operations |
| Short Description | Generate experience letter |
| Due Date | Based on SLA |
| Order | After Approval |

**Result:** the HR task is created and assigned automatically.

### Activity Set (Execution Order)

Service Activities run based on **Order**; lower order executes first.

| Order | Activity |
|---|---|
| 10 | Manager Approval |
| 20 | Generate Experience Letter Task |
| 30 | Upload Document Task |
| 40 | Close Case |

### Using Flow Designer in HRSD (recommended)

Instead of multiple activities, you can use one **Flow** activity.

1. Create a flow: Flow Designer → New Flow.
2. **Trigger:** HR Case Created.
3. **Condition:** HR Service = Request Experience Letter.
4. **Actions:** Ask for Approval; Create HR Task; Update Case State; Notify Employee.
5. Attach the flow to a Service Activity (Type = Flow).

### Best practices (real projects)

- Always use Service Activities.
- Prefer Flow Designer for complex logic.
- Reuse activities across HR services.
- Keep approval conditions minimal.
- Use case templates for consistency.

[Back to top](#table-of-contents)

---

## 9. HR Criteria and HR Templates

Topics: what HR criteria are, where they are used, creating HR criteria with groups/users/roles, what an HR template is, where it is used, creating one.

**HR criteria** define the audience for HR content, services or cases. You can make information available to, or create an HR case for, specific groups, individuals, or all employees. HR criteria use the standard platform **condition builder** to support complex conditions that control the Employee Center content employees see when logged in.

**HR template:** automatically populates fields on an HR case or task form. You can configure HR templates for:

- HR services
- HR catalog items
- HR service activities
- Lifecycle event activities

**Configure an HR case template on an HR service**

1. Navigate to **All > HR Administration > HR Services > HR Service Configuration**.
2. Open the HR service.
3. Open the HR template for that service.
4. Fill in the fields on the form, as appropriate.
5. Click **Submit** or **Update** on the HR template form.
6. Click **Update** on the HR service form.

**Edit the HR case template for an HR catalog item**

1. Navigate to **All > HR Administration > HR Services > Manage HR Catalog**.
2. Hover over the HR catalog item and click **Edit Case Template**.
3. Fill in the fields on the form, as appropriate.
4. Click **Update**.

[Back to top](#table-of-contents)

---

## 10. HR Catalog, Record Producers and Service Activities

**HR service catalog management:** the catalog gives employees HR services they can request directly from HR, such as adding beneficiaries to a benefits plan, requesting a travel visa, or inquiring about a paycheck discrepancy. Employees access it from a self-service portal such as the Employee Service Center. You manage the setup and categorization of the catalog and configure individual catalog items.

**Categorization:** HR catalog items are organized into categories, for example benefits, employee data management, employee relations, general, HR systems, payroll, talent management and visas. You can create or modify categories, and assign or remove items from a category.

**HR catalog item configuration:** HR catalog items (also known as **record producers**) are the HR services made available for employees to request. You configure the required fields and how the item appears in the catalog, and you can configure an HR case template to populate case fields automatically.

- Create an HR catalog item
- Edit the record producer
- Edit the HR case template
- HR criteria in catalog item

### Service Activities (very important)

**What it is:** Service Activities define the tasks, approvals and steps HR must complete after a case is created.

**Purpose:** automate HR work; ensure consistent processing; track SLA and progress.

**Examples:** Manager Approval, Document Verification, Background Check, Payroll Update, Final Confirmation.

**Navigation:** HR Service Management → Administration → Service Activities

| Type | Description |
|---|---|
| Task | Creates HR tasks |
| Approval | Manager / HR approval |
| Flow | Triggers Flow Designer |
| Manual | Informational steps |

### End-to-end flow

```
Employee
  ↓
HR Catalog (Employee Service Center)
  ↓
HR Record Producer (Form)
  ↓
HR Case Created
  ↓
Service Activities Triggered
  ↓
HR Tasks / Approvals
  ↓
Case Closed
```

**Example: Experience Letter Request**

1. **HR Catalog:** service name *Request Experience Letter*.
2. **Record Producer:** form fields Employee ID, Date Range, Purpose.
3. **HR Case:** auto-created and assigned to HR Operations.
4. **Service Activities:** Manager Approval; HR Task: Generate Letter; HR Task: Upload and Notify Employee.

### When to use what

| Requirement | Use |
|---|---|
| Employee request | HR Catalog |
| Custom input form | HR Record Producer |
| HR work tracking | HR Case |
| Process automation | Service Activities |

### Best practices (real-time projects)

- One record producer per HR service.
- Reuse Service Activities across services.
- Keep forms simple for employees.
- Use Flow Designer inside Service Activities.
- Use HR templates for consistency.

[Back to top](#table-of-contents)

---

## 11. E-Signature Upload

E-Signature in HRSD lets employees electronically sign HR documents (offer letters, policies, NDAs, consent forms) and upload and store them securely in HR cases.

### What is E-Signature in HRSD?

- Digital signing of HR documents.
- Legal compliance and audit trail.
- Secure storage against HR cases.
- Faster, paperless HR processing.

Commonly used in onboarding, offboarding and policy acknowledgement.

### Supported ways to enable E-Signature

**Option A: Native (basic).** Employee signs outside (PDF tool) and uploads to ESC. Manual upload, no true digital certificate. ✅ Simple, ❌ not legally strong.

**Option B: Integrated e-signature (recommended).** ServiceNow integrates with **DocuSign, Adobe Acrobat Sign, HelloSign**. ✔ Legally compliant, ✔ audit trail, ✔ automated signing flow.

### Architecture overview

```
HR Case Created
  ↓
HR Task / Service Activity
  ↓
E-Signature Request Sent
  ↓
Employee Signs Document
  ↓
Signed Document Attached
  ↓
HR Case Updated
```

### Configure e-signature integration (DocuSign example)

1. **Activate the plugin:** System Definition → Plugins → activate *DocuSign for ServiceNow (HR compatible)*.
2. **Integration setup:** DocuSign → Configuration; set API credentials, callback URL, authentication method.
3. **HR document template:** HR Service Management → Administration → HR Documents; upload the offer letter / policy PDF and define signature fields.

### Trigger e-signature from an HR Service Activity

**Navigation:** HR Service Management → Administration → Service Activities → New

| Field | Value |
|---|---|
| Type | Flow |
| HR Service | Offer Letter / Policy Acknowledgement |
| Flow | Send for E-Signature |

### Flow Designer: e-signature flow

1. **Trigger:** HR Case Created.
2. **Condition:** HR Service = Offer Letter.
3. **Actions:** Send DocuSign envelope; Wait for signature; Attach signed document; Update case state.

Fully automated.

### Employee experience (ESC)

`/esc → My Requests → Open HR Case`. The employee can view the document, click **Sign**, complete the signature and track status. The signed document appears under **Attachments**.

### Store signed document securely

Signed documents are stored as an attachment on the HR case, or in HR Document Management. They follow HR data segmentation and are accessible only to authorized HR roles.

### Audit and compliance

E-signature provides timestamp, IP address, signer identity and document hash. Audit logs are in **Integration Logs / DocuSign Logs**.

### Common HRSD e-signature use cases

| Use case | Signature |
|---|---|
| Offer Letter | Employee |
| Policy Acknowledgement | Employee |
| NDA | Employee + Employer |
| Exit Clearance | Employee |
| Consent Forms | Employee |

### HR e-signature (native scoped app)

E-signature is a scoped application that enables users to sign managed documents, knowledge articles or HR document templates with a **typed or drawn e-signature, their credentials, or as an acknowledgment**, from any desktop or mobile device.

- The e-signature plugin `com.snc.esign` activates the system property `sn_esign.min_admin_count`. It prevents you from deleting your only e-signature admin user by requiring a minimum number of active users with this role (**default is two**).

**How to use e-signature templates with HR tasks**

- Users with the HR administrator (`sn_hr_core.admin`) or basic (`sn_hr_core.basic`) role can create e-signature templates.
- Each template pairs a **document type to sign** (managed document, knowledge article, or HR document template) with a **signature type** (signature, credential, or acknowledgment). Example: an NDA template requiring a typed or drawn signature.
- Go to table `sn_esign_configuration_list` to find the relevant document.
- To create a document, navigate to `sn_esign_configuration` and find the **Managed document** field.
- To attach a document to the Managed document field, go to the `dms_document` table and create one or find an existing one.

**Create an HR task with e-signature (walkthrough)**

1. Create an HR service "Payroll issues" and select all the fields.
2. Check that the service activity related tab is enabled.
3. Create three tasks: (3a) Approval, (3b) Payroll Issue task 1, (3c) HR Payroll task 2. For the child tasks, create a child task template.

[Back to top](#table-of-contents)

---

## 12. Transfer Case and Bulk Case Creation

### Transfer Case

**What is a transfer case?** Moving an HR case from one HR service, assignment group or HR team to another **without losing history, SLA or audit data.**

**When it is used**

- Wrong HR service selected by the employee.
- Case routed to the incorrect HR group.
- Organizational changes.
- A specialized HR team takes over.

**How to transfer an HR case**

*Method 1: Transfer Case action (recommended).* Navigation: HR Service Management → Cases → HR Cases.

1. Open the HR case.
2. Click **Transfer Case**.
3. Select the new HR service (optional), new assignment group and reason for transfer.
4. Submit.

**Result:** case history is retained; new service activities can trigger; SLA continues (or restarts, based on configuration).

*Method 2: Manual update (not recommended).* Changing the assignment group or HR service directly risks missing activities.

**What happens in the background:** case ownership changes; transfer activity is logged; new service activities may start; previous tasks can be cancelled (configurable).

**Transfer methods (base system)**

| | **Reclassify** | **Standard** |
|---|---|---|
| Case number | Does **not** change | **Changes**; both numbers appear on the case |
| Links | Redirect to the new case for *Opened for* and *Subject person* | Do not redirect from original to new case |
| Notification emails | Not sent | Subject person receives an email with closed case and transferred case information |

**Common to both:**

- Fields from the current case are copied to the new case. The `sn_hr_core.transfer_case.ignored_fields` system property can list fields that are not copied.
- Work notes are copied from the original case.
- Attachments are copied and moved.
- Previous interaction records are copied and moved.

The system property `sn_hr_core.reclassify_default_transfer` determines the default method after upgrade. Often a case opens as a *General Benefits Inquiry* and, after investigation, is transferred to the applicable HR service.

**Configure HR transfer case**

- **Role required:** `sn_hr_core.admin`.
- If you create a transfer case configuration, you must also create a **script include** that implements the `sn_hr_core` HR case transfer extension point.
- Existing examples: `hr_TransferCase` (lists fields ignored in transfer and acts as a utility for the other two), `ReclassifyCaseTransfer`, `StandardCaseTransfer`.

**Best practices:** always use the Transfer Case action; use case templates for bulk creation; communicate the transfer reason; test SLA behavior after transfer; avoid manual reassignment.

### Bulk Case Creation

**What is it?** Creating multiple HR cases at once for many employees, without creating the same case repeatedly.

**Common use cases:** policy acknowledgment rollout, organization-wide announcements, mass onboarding/offboarding, payroll correction for many users.

**Methods**

1. **HR Case Bulk Creation tool (best practice).** Navigation: HR Service Management → Administration → Bulk Case Creation. Select the HR service; choose a case template; select employees (user list, department, location or uploaded file); submit. **Result:** one HR case per employee with the same service and activities, plus automated tasks and approvals.
2. **Data import (Import Set).** System Import Sets → Load Data. Use for large volumes, external system data or scheduled uploads.
3. **Flow Designer / script (advanced).** For custom logic, event-based creation or integrations.

**What gets created for each employee:** HR case, service activities, HR tasks, approvals (if configured).

**Example: policy acknowledgment**

1. HR creates a Policy Acknowledgment HR service.
2. Uploads the policy document.
3. Uses Bulk Case Creation.
4. Selects all active employees.
5. HR cases are created automatically.
6. Employees acknowledge in ESC.
7. HR tracks the completion report.

**Security and data segmentation:** each employee sees only their own case; HR sees all cases; no cross-user visibility. Controlled by HR Service Management → Administration → Data Segmentation.

**Segments and roles**

- A **segment** categorizes and groups employees for whom cases are being created, so HR can efficiently handle a subset sharing characteristics.
- **Role required:** HR admin (`sn_hr_core.admin`). Previously `sn_hr_core.case_writer`, but because bulk creation involves a large number of cases, the requirement changed to HR admin. To let case writers create bulk cases, update the ACL for table **Bulk Case Request** (`sn_hr_core_bulk_case_request`).
- Bulk cases can be created for all active users. For users without an HR profile (such as new hires), an HR profile is created automatically.

**Examples:** an annual recertification task to a group; onboarding multiple new hires; managing a payroll issue affecting multiple employees.

**Bulk case process**

1. **Create and save a bulk case request.** It starts the process and holds basic information about the cases. You must create and associate **at least one user segment group** to it.
2. **Create one or more user segment groups.** They specify values for a group of users, can define different values for different groups, and let admins verify correctness before executing. After creation you can see the number and users targeted and can remove specific users.
3. If the groups look accurate, **create cases.**

**Key points**

- **Capacity:** designed for up to **10,000 basic HR cases**, or **1,000 cases using workflows** such as Lifecycle Event cases. Performance declines beyond that.
- **Previous releases:** complete or close all bulk case requests from previous releases; incomplete ones cannot be completed in the current version. Completed ones may have non-editable fields and no visible segments.
- **Async:** bulk case requests are queued and updated asynchronously in the background.

[Back to top](#table-of-contents)

---

## 13. Employee Center Pro and ESC Ticket Page Configuration

### Employee Center Pro

Employee Center Pro expands the portal beyond service delivery with employee communication and engagement tools, including content publishing campaigns and employee communities. It is a modern, multi-department employee destination site that drives engagement and productivity.

- **Unified portal experience:** reduces time employees spend looking for services, updates and information.
- **Extensible service delivery platform:** scale from a single to a multi-department use case, with a best-in-class home page and specialized widgets for multi-department tasking.
- **Employee engagement and communication:** employees stay informed on news and events, engage with coworkers and leaders, receive targeted communications and find cross-enterprise answers.

**Configuration categories** (each has its own landing page):

| Category | What it covers |
|---|---|
| Admin configurations | Install plugins, assign roles, customize the portal with a theme, footer and right-to-left language support. |
| Browse experience | Make it easy for employees to navigate and discover content. |
| Search experience | Enhance the search experience. |
| Task management | Enhance productivity and drive portal consolidation. |
| Employee communications | Inform and engage employees. |
| Continuous improvement | Improve efficiency and performance through feedback and analytics. |

**Plugins required for Employee Center Pro**

- Employee Center (`sn_ex_sp`)
- Employee Center Pro (`sn_ex_sp_pro`)
- Enterprise Service Management Integrations Framework (`sn_hr_integ_fw`)
- Employee Center Core (`sn_hr_sp`)
- Employee Experience Taxonomy (`sn_ect`)
- Employee Experience Foundation (`sn_ex_emp_fd`)
- Content Publishing (`sn_sd`)
- Content Experiences (`sn_ca`)

**Configure the ESC portal**

- Customize the Employee Center portal theme.
- Employee Center global header.
- Footer configuration.
- Right-to-left language support (not required).
- Customize portal pages such as the home page: **Service Portal > Pages**. The Employee Center Pro home page is `ec_pro_home`.

**Customize the portal theme** (role: admin): the default **EC Theme** ships with Employee Center and gives a consistent experience across all pages. Navigate to **All > Service Portal > Themes** and search for *EC Theme*. Use the default theme for better visual and usability experience, though you can choose others. To preview: **All > Service Portal > Portals > Employee Center**, click **Try it**, then **Update**. *(The record lives in the Employee Center Core application while Global is the current application; switch application scope to edit it.)*

**Global header:** the first row of the portal header, with menus for **My To-dos, My Requests, More menu, user profile and Tours**. You can enable or disable these options via additional options in JSON format on the Employee Center menu record.

**Footer configuration:** Employee Center and Pro include a default footer, configurable to show corporate office address, email address, terms and conditions, privacy policy and social media handles.

### Browse experience setup

1. Define the content hierarchy via the **taxonomy** (Unified Taxonomy for Employee Center).
2. Configure how the taxonomy appears on portal pages: **Dynamic topic pages**.
3. Configure navigation menus: **Mega menu** and **Advanced Portal Navigation**.
4. Configure out-of-the-box widgets (optional): browse experience widgets.

**Unified Taxonomy:** a collection of hierarchical topics that brings together different content types (requests, articles, quick links, learning content, employee communications). It is built on three models:

- **Content taxonomy (data model):** the framework against which content is mapped; the top three levels of the topic (for example IT > Hardware > Printers).
- **Content authoring (taxonomy management):** lets content managers and contributors tag content against the taxonomy and manage it.
- **Taxonomy for employee content:** the actual content mapped to the taxonomy, including default content shipped by IT, HRSD and Legal.

**Taxonomy in ServiceNow** is a system that groups content into a hierarchy based on topics. It organizes requests, articles and communications, and helps determine navigation menus and subpages for the Employee Service Center.

**Dynamic topic pages:** topic pages dynamically configured from content tagged to a unified taxonomy across all departments. They are separate portal pages that open when a user clicks a topic from the mega menu or popular topics (not part of the home page), and give a consistent experience across topics and channels in the web and Now Mobile app. You can also create content from **Portal content**.

**Mega menu** (role: admin): lets users navigate to a topic and its child topic. The taxonomy associated with the Employee Center portal record determines the mega menu and its items; topics and child topics in the taxonomy appear as mega menu items by default. Taxonomy topics become available after installing and activating the **Employee Experience Taxonomy** plugin (`sn_ect`). The base taxonomy, **Employee**, is installed with it. The default Employee taxonomy includes **IT, HR, Workplace, Legal and Procurement** mega menus, showing three levels: root topic, child topic 1, child topic 2 (for example IT > Hardware > Printers).

**Create a root topic** (role: `taxonomy_admin`; user criteria: Taxonomy Manager, Taxonomy Contributor, Topic Manager, Topic Contributor): **All > Content Taxonomy > Topics > Create** and save the form, associating it with the cloned or new taxonomy. With many topics, set up user criteria for them.

**Create a child topic:** **All > Content Taxonomy > Topics >** open a parent topic **> Child topic** related list **> Create new**.

**Advanced Portal Navigation (APN):** helps design intuitive navigation for better information architecture and topic discovery. You can manage topics, SP menu items, or a mix of both and design the primary navigation menu based on employee needs and content priorities.

> **Note:** when APN is enabled, you manage root-level topics and menu items from this table itself, not from the taxonomy or SP menu items.

### ESC Ticket Page Configuration (HRSD)

The ESC Ticket Page is where employees **view, track and interact with their HR cases** after submission. It shows:

- Case number and status
- HR service name
- Assigned HR group
- Comments / messages
- Attachments
- Approvals
- Case timeline

It is secure (HR ACLs applied) and an employee sees only their own cases.

**Where it is configured:** ESC is built on Service Portal, main portal `/esc`.

| Area | Navigation |
|---|---|
| Service Portal | Service Portal → Service Portal Configuration |
| Pages | Service Portal → Pages |
| Widgets | Service Portal → Widgets |
| HR case table | `sn_hr_core_case` |
| HR config | HR Service Management → Administration |

**Ticket page (case page):** `esc_ticket_page`. Navigation: Service Portal → Pages → `esc_ticket_page`.

**Key widgets on the ESC ticket page**

| Widget | Purpose |
|---|---|
| Case Header | Shows case number, service, state |
| Case Details | Case fields |
| Activity Stream | Comments and work notes |
| Attachments | Upload / view files |
| Approval Widget | View and approve |
| Case Timeline | Track progress |
| Related Records | Tasks and approvals |

Widgets control what the employee can see.

**Configure visible fields:** HR Service Management → Administration → **Case Field Configurations**. Configure fields visible to the employee, fields visible to HR only, and read-only vs editable. Example: Short Description (read-only), State (read-only), Attachments (editable), Comments (editable).

**Configure case status and labels (user-friendly):** HR Service Management → Administration → **Case Statuses**.

| Internal state | ESC display |
|---|---|
| New | Submitted |
| Work in Progress | In Review |
| Awaiting Info | Waiting for You |
| Closed Complete | Completed |

**Approvals on the ticket page:** they appear automatically when the approval is linked to the HR case and the approver is the logged-in user. Widget: Approval Widget. Sources: Service Activity → Approval; Flow Designer → Ask for Approval.

**Comments and communication**

| Type | Visible to employee |
|---|---|
| Additional comments | ✔ Yes |
| Work notes | ❌ No |

Controlled by widget config and HR ACLs.

**Attachments:** employees can upload documents and view HR-uploaded files. Controlled by Case Field Configuration and attachment security rules.

**Security and ACLs (very important):** HRSD uses **Data Segmentation**, ensuring an employee sees only their own case, sensitive HR info is hidden, and HR agents see assigned cases. Navigation: HR Service Management → Administration → Data Segmentation.

**Customize layout (optional):** Service Portal → Pages → `esc_ticket_page` → Layout Container. You can add/remove widgets, reorder sections and change widget options. **Avoid heavy customization in PROD.**

**End-to-end flow (employee view)**

```
Employee submits HR request
  ↓
HR case created
  ↓
Employee opens ESC
  ↓
My Requests → Select Ticket
  ↓
ESC Ticket Page displays
  ↓
Employee tracks status, uploads docs, approves
```

**Best practices:** use default widgets where possible; keep UI simple; use friendly status labels; never expose work notes; test with the employee role (`sn_hr_core.employee`).

[Back to top](#table-of-contents)

---

## 14. HR Reports and Dashboards

The HR Dashboards and Reports application gives the HR organization visual representations of accountability, progress, performance metrics, employee satisfaction and HR case status in the scoped version of HR.

**Tools available**

- **Dashboards:** Overview, Case and Manager dashboards (a type of homepage containing reports and widgets). Click a listed record or report segment to see details.
- **HR reports:** several are predefined and you can create more.
- **Survey response charts and detail** for the HR case satisfaction survey or other surveys you create.
- You can customize any accessible dashboard, for example an HR benefits administrator adds a report to the Overview dashboard showing the status of cases in the Benefits service.

**HR Dashboards and Reports provide:** Overview, Manager Dashboard, HR Case Dashboard, Reports, Satisfaction Survey Scorecard.

**Migration to Platform Analytics Workspace:** the HR Overview Dashboard, Manager Dashboard, HR Case Dashboard and SLA Dashboard have been migrated to **Platform Analytics Workspace**. Select the **Analytics Center** icon to open them. Install Platform Analytics Workspace from the ServiceNow Store and have the `pa_viewer` role.

**Overview dashboard (default reports)**

- *HR Cases Opened in the Last 6 Months by Topic Category:* color-coded bar chart.
- *Active HR Case Breakdown by Topic Category:* bar chart by active HR cases and assigned HR agent.
- *Active HR Cases by Topic Category:* pie chart.
- *Active HR Cases Pivot Table by Topic Category:* cross-references the assigned HR agent by topic category with the number of cases.

**Manager dashboard:** monitors HR team effectiveness through metrics such as the state of assigned cases by HR agent and SLA elapsed time percentage.

**HR Case dashboard:** a central place for HR users to monitor ongoing operations. It shows Open/Assigned cases, High Priority cases and cases breached by SLA, plus cases by service and cases assigned to you.

### Types of HR reports

| Type | Used by | Examples |
|---|---|---|
| **A. Operational reports** (daily HR work) | HR agents and managers | Open HR cases by assignment group; cases awaiting employee response; overdue HR tasks; cases by state. Tables: `sn_hr_core_case`, `sn_hr_core_task` |
| **B. Management / KPI reports** | HR leadership | Average resolution time; SLA compliance %; case volume trend (monthly); first contact resolution |
| **C. Compliance and audit reports** | Legal and policy compliance | Closed cases by HR service; access to sensitive HR services; data segmentation audit |

### Creating HR reports (step by step)

Navigation: **Reports → Create New**

1. **Choose table:**

   | Report type | Table |
   |---|---|
   | HR Case | `sn_hr_core_case` |
   | HR Task | `sn_hr_core_task` |
   | SLA | `task_sla` |

2. **Apply filters** (examples): `Active = true`; `HR Service is not empty`; monthly cases: `Opened on → This month`.
3. **Select report type:**

   | Report | Use |
   |---|---|
   | Bar / Pie | Case distribution |
   | Line | Trend over time |
   | List | Detailed view |
   | Pivot | SLA and workload |

4. **Group by:** HR service, assignment group, state, priority.
5. **Save and share:** set visibility (HR roles only); schedule the report if required.

### Common HRSD reports (real projects)

- **HR case volume by service:** table `sn_hr_core_case`, group by HR Service.
- **SLA compliance report:** table `task_sla`, filter Task type = HR Case, group by SLA Definition.
- **HR agent workload:** table `sn_hr_core_case`, group by Assigned to.
- **Aging report:** filter Opened > 7 days ago and State != Closed.

### HR dashboards

Dashboards show multiple HR reports in one view. Navigation: **Self-Service → Dashboards**, or **Performance Analytics → Dashboards** (if PA is enabled).

| Dashboard | For | Widgets |
|---|---|---|
| **HR Operations** | HR agents and managers | Open HR cases; overdue tasks; approvals pending; SLA breaches |
| **HR Leadership** | CHRO / HR head | Monthly case trend; top 5 HR services; SLA compliance %; average resolution time |
| **Compliance** | Compliance teams | Closed cases by service; sensitive HR service access; audit logs |

**Create an HR dashboard:** Self-Service → Dashboards → Create New. (1) Name it, for example *HR Operations Dashboard*. (2) Add reports as widgets. (3) Arrange layout. (4) Set access control. Each widget = one report.

### Performance Analytics (advanced)

If the PA plugin is enabled, benefits include trend analysis, historical data, and targets and breakdowns. Common HR PA indicators: average case resolution time; cases opened vs closed; SLA compliance %. Navigation: Performance Analytics → Indicators.

### Security and data visibility (very important)

HR reports follow HR Data Segmentation rules: employees see only their data; HR agents see assigned cases; managers see team data. Controlled by HR Service Management → Administration → Data Segmentation.

**Best practices:** use role-based access; separate operational and leadership dashboards; use PA for trend analysis; avoid overloading dashboards; mask sensitive data.

[Back to top](#table-of-contents)

---

## 15. Email Configuration (Inbound and Outbound)

Email plays a key role in HRSD for case creation, communication and notifications, while maintaining confidentiality.

### A. Inbound email configuration (HR case creation)

Inbound email lets employees send an email to HR, which automatically creates an HR case.

**Configuration path:** System Policy → Email → Inbound Actions

```
Employee sends email to HR mailbox
  ↓
Inbound Email Action triggers
  ↓
HR Case is created
  ↓
Assigned to HR group
  ↓
Employee receives confirmation
```

**Key configuration (HR inbound action)**

| Field | Value |
|---|---|
| Name | Create HR Case from Email |
| Table | `sn_hr_core_case` |
| Condition | Email sent to hr@company.com |
| HR Service | General HR Inquiry |
| Assignment Group | HR Operations |
| Opened for | Email sender |

**Best practices:** use a dedicated HR email address; map email body to case description; apply data segmentation; avoid exposing work notes.

### B. Outbound email configuration (notifications)

Outbound email sends notifications to employees, managers and HR agents.

**Configuration path:** System Notification → Email → Notifications

| Notification | Trigger |
|---|---|
| Case Created | HR case inserted |
| Approval Required | Approval requested |
| Awaiting Info | HR needs employee input |
| Case Closed | HR case closed |
| Document Signed | E-signature completed |

**Notification best practices:** use email templates; mask sensitive data; use simple language; avoid attachments unless required.

### C. HR email security (very important)

HRSD uses HR Data Segmentation:

- Employee sees only their case.
- No CC/BCC exposure.
- Emails are logged on the case.
- Secure reply-to configuration.

Controlled via HR Service Management → Administration → Data Segmentation.

[Back to top](#table-of-contents)

---

## 16. HR Integration

**What is HR integration?** HR integrations connect ServiceNow HRSD with external HR systems to automate employee lifecycle events and reduce manual work.

**Common HR systems integrated with HRSD**

| System | Purpose |
|---|---|
| Workday | Core HR data |
| SAP HCM / SuccessFactors | Payroll and employee data |
| Oracle HCM | HR master data |
| Active Directory | User provisioning |
| DocuSign | E-signature |
| Payroll systems | Compensation updates |

**Types of HR integrations**

- **A. Inbound** (external HR systems → ServiceNow): new hire data, job change, termination, manager change.
- **B. Outbound** (ServiceNow → external systems): access requests, payroll updates, IT provisioning.

### HR Lifecycle Events (core integration concept)

Predefined employee lifecycle triggers in HRSD. Examples: **Hire, Onboard, Transfer, Promote, Terminate, Rehire.**

```
HR system sends event (Hire)
  ↓
ServiceNow HR Lifecycle Event triggered
  ↓
HR Case(s) created automatically
  ↓
Tasks assigned to HR / IT / Facilities
```

### Integration methods used in HRSD

| Method | Use case |
|---|---|
| REST APIs | Real-time integration |
| Import Sets | Bulk data load |
| Scheduled Jobs | Periodic sync |
| MID Server | Secure on-prem integration |

### Example: Workday → ServiceNow HRSD

1. Workday sends new hire data.
2. ServiceNow receives it via REST API.
3. Lifecycle event: *Onboard Employee*.
4. HR cases are auto-created.
5. IT and Facilities tasks are triggered.

### Email plus integration combined example

**Scenario:** an employee joins the company.

- Workday sends the new hire record.
- HRSD creates an onboarding case.
- HR tasks are generated.
- Outbound emails notify the employee and manager.
- The employee replies via email and the case is updated.

### Best practices

- Separate HR email mailbox.
- Use lifecycle events for automation.
- Prefer REST APIs over manual imports.
- Mask sensitive fields in emails.
- Log all email communication on the case.

[Back to top](#table-of-contents)
