# Day 06 - Service Catalog Fundamentals

**Date:** 15 July 2026

---

## Introduction

Today's session focused on one of the most important modules in ServiceNow—the **Service Catalog**. Until now, the topics covered during the internship were primarily administrator-oriented, such as CMDB, Knowledge Management, Import Sets, and platform configuration. In this session, the focus shifted towards the **end-user experience** and how organizations provide a centralized self-service portal for requesting services.

The Service Catalog is much more than a list of services. It provides a structured and standardized way for employees to request hardware, software, access, support, and many other business services from a single location. Behind the scenes, ServiceNow automatically manages approvals, task assignments, notifications, and fulfillment using workflows and automation.

Throughout today's session, I learned the overall architecture of the Service Catalog, its key features, how requests flow from submission to fulfillment, and later performed hands-on exercises by creating a custom catalog, category, catalog item, and publishing them to the Service Portal.

---

## What is Service Catalog?

The **Service Catalog** is a centralized self-service application within ServiceNow that enables users to browse, request, and order enterprise services through a single, user-friendly interface. These services may include hardware, software, access requests, IT support, HR services, facilities requests, and many other business offerings.

Although users simply submit requests through an easy-to-use portal, ServiceNow automatically handles the entire fulfillment process in the background using approvals, workflows, flows, notifications, and task assignments.

In simple terms, the Service Catalog acts as the bridge between end users who need services and the teams responsible for delivering those services.

> **Interview Definition:**
> **Service Catalog** is a centralized self-service portal in ServiceNow that allows users to request enterprise services, hardware, software, and support through standardized catalog items, while automated workflows manage approvals, fulfillment, and tracking.

---

## Why is the Service Catalog Important?

In most organizations, employees frequently require services from multiple departments.

For example:

- IT provides laptops, software installations, VPN access, and email accounts.
- HR manages onboarding, leave requests, and employee documentation.
- Facilities handles meeting room bookings, parking requests, and access cards.
- Finance may provide reimbursement or procurement-related services.

Without a centralized platform, employees would need to contact each department separately through emails, phone calls, or individual portals. This often leads to delays, inconsistent processes, and difficulty tracking requests.

The Service Catalog solves this problem by providing a **single destination** where users can discover, request, and monitor services regardless of which department fulfills them.

This improves both the employee experience and operational efficiency.

---

## Key Benefits of the Service Catalog

Some of the major advantages include:

- Centralized self-service experience.
- Standardized service request process.
- Reduced manual communication between users and support teams.
- Automated approvals and fulfillment.
- Better visibility into request status.
- Consistent experience across multiple departments.
- Faster service delivery through automation.
- Improved user satisfaction.

---

## Key Features of the Service Catalog

### 1. One-Stop Shopping

One of the biggest advantages of the Service Catalog is that it provides a **single location** where users can browse and request all available enterprise services.

Instead of navigating multiple applications or contacting different departments individually, users simply visit the Service Catalog and choose the services they require.

Examples include:

- Laptop Request
- Desktop Request
- Software Installation
- VPN Access
- Password Reset
- Employee ID Card
- New Employee Onboarding
- Printer Request
- Office Accessories

Everything is available from one centralized location.

This concept is often referred to as **One-Stop Shopping**, because it resembles an online shopping website where everything can be ordered from a single place.

### 2. Organized by Categories

As organizations grow, the number of available services also increases.

Instead of displaying hundreds of Catalog Items in one long list, ServiceNow organizes them into **Categories**.

Categories improve navigation and help users quickly locate the services they need.

Some common categories include:

| Category | Example Services |
|-----------|------------------|
| Hardware | Laptop, Desktop, Keyboard, Mouse |
| Software | Microsoft Office, Adobe Reader, Visual Studio Code |
| IT Services | VPN Access, Password Reset, Email Configuration |
| Human Resources | Leave Request, Employee ID Card, Onboarding |
| Facilities | Meeting Room Booking, Parking Pass, Building Access |
| Finance | Reimbursement, Procurement Requests |

This hierarchical structure makes the Service Catalog much easier to browse. Instead of searching through every available service, users simply select the relevant category first.

### 3. Service Desk Access

The Service Catalog also serves as a direct connection between end users and the **Service Desk**.

Users can raise requests, report issues, and submit support requests without needing to know which team is responsible.

Once a request is submitted, ServiceNow automatically performs tasks such as:

- Routing the request to the correct team.
- Initiating approvals.
- Assigning fulfillment tasks.
- Sending notifications.
- Tracking request progress.
- Closing the request after completion.

This creates a seamless end-to-end service management process. Instead of manually coordinating between teams, ServiceNow automates the entire lifecycle.

### 4. Multiple Catalogs

Large organizations often have multiple departments, each offering different services.

To support this, ServiceNow allows multiple independent **Service Catalogs** within the same instance. Each department can maintain its own catalog while still using the same platform.

For example:

**IT Catalog**
- Laptop Request
- Software Installation
- VPN Access
- Password Reset

**HR Catalog**
- Leave Request
- Employee Onboarding
- Employee ID Card
- Employment Verification

**Facilities Catalog**
- Meeting Room Booking
- Parking Pass
- Office Access Card
- Workspace Allocation

This separation keeps services organized and allows organizations to present different catalogs to different groups of users.

---

## Creating a Catalog and Category

After understanding the theory behind multiple catalogs, the next step was to create a custom Service Catalog inside the developer instance. This practical exercise demonstrated how administrators build the structure that users eventually interact with through the Service Portal.

### Creating a New Catalog

Navigation followed:

```text
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Catalogs
```

From **Maintain Catalogs**, a new catalog was created. The catalog name used during the demonstration was:

```text
Zebronics Catalog
```

After entering the required details, the catalog was created successfully. This catalog now acts as a container that can hold multiple categories and Catalog Items.

### Creating a Category

Once the catalog was created, the next step was organizing it using Categories.

Navigation:

```text
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Categories
```

A new Category record was created with the following values:

| Field | Value |
|--------|-------|
| Title | Hardware |
| Catalog | Zebronics Catalog |

No additional fields were configured during the demonstration. After selecting the catalog and entering the category name, the record was saved using **Submit**.

This created a **Hardware** category inside the **Zebronics Catalog**. Later, Catalog Items such as keyboards, laptops, printers, and other hardware requests can be placed inside this category.

### Understanding the Relationship

One important concept demonstrated during the lab was the relationship between Catalogs and Categories.

A Catalog acts as the highest level of organization. Inside each Catalog, multiple Categories can be created. Each Category then contains multiple Catalog Items.

The structure looks like this:

```text
Zebronics Catalog
│
├── Hardware
│      ├── Laptop Request
│      ├── Keyboard Request
│      ├── Mouse Request
│      └── Printer Request
│
├── Software
│      ├── Microsoft Office
│      ├── VS Code
│      └── Adobe Reader
│
├── Accessories
│
└── Services
```

This hierarchical structure keeps the Service Catalog organized and easy to navigate, especially in large enterprise environments.

### Real-World Example

Consider an employee who needs a new mechanical keyboard. Instead of searching through hundreds of available services, the employee simply follows this path:

```text
Service Catalog
        ↓
Zebronics Catalog
        ↓
Hardware
        ↓
Mechanical Keyboard
```

This makes the ordering experience intuitive and significantly reduces the time required to locate services.

### Memory Tip

Think of the Service Catalog like an online shopping website.

```text
Amazon
│
├── Electronics
├── Fashion
├── Grocery
└── Books
```

Similarly,

```text
Service Catalog
│
├── Hardware
├── Software
├── HR Services
└── Facilities
```

And just as products are placed inside shopping categories, **Catalog Items** are placed inside **Categories**, which are organized within a **Catalog**.

---

## Service Catalog Request Lifecycle

After understanding what the Service Catalog is and how Catalogs and Categories organize services, the next concept covered was **how a request actually travels through ServiceNow** once a user places an order.

Although the user only sees a simple request form in the Service Portal, ServiceNow performs several actions in the background to ensure every request is tracked, approved, assigned, and fulfilled correctly.

Every request follows a structured lifecycle, allowing organizations to monitor its progress from the moment it is submitted until it is completed.

```text
Catalog Item
      ↓
Shopping Cart
      ↓
Request (REQ)
      ↓
Requested Item (RITM)
      ↓
Catalog Task (SCTASK)
      ↓
Fulfillment
      ↓
Closure
```

Each stage creates its own record inside ServiceNow, making every request traceable throughout its lifecycle. This structured approach ensures that approvals, assignments, notifications, and fulfillment activities are properly managed without losing track of the request.

### Step 1 — Catalog Item

The lifecycle begins when a user selects a **Catalog Item**. A Catalog Item represents an individual service or product available for request through the Service Catalog.

Examples include:

- Laptop Request
- Desktop Request
- VPN Access
- Microsoft Office Installation
- Printer Request
- Employee ID Card
- Email Account Creation

Each Catalog Item is designed to collect only the information required to fulfill that specific request. For example, a Laptop Request may ask for:

- Employee Name
- Department
- Laptop Model
- Business Justification
- Required Date

Once the user selects the desired Catalog Item, the ordering process begins.

### Step 2 — Shopping Cart

Instead of submitting each request individually, ServiceNow provides a **Shopping Cart**, similar to an e-commerce website. The Shopping Cart temporarily stores all selected Catalog Items before submission.

Users can:

- Add multiple Catalog Items.
- Remove unnecessary items.
- Review their selections.
- Submit all requests together.

For example, a new employee may require several services at once. The Shopping Cart could contain:

```text
Shopping Cart
✓ Laptop Request
✓ Microsoft Office
✓ VPN Access
✓ Employee ID Card
```

Rather than submitting four separate requests, everything can be ordered together. This improves the user experience while reducing repetitive work.

### Step 3 — Request Management

Once the Shopping Cart is submitted, ServiceNow automatically begins the request management process. Behind the scenes, the platform creates dedicated records to track the request.

The request is divided into two major levels:

- Request (REQ)
- Requested Item (RITM)

This separation makes it easier to monitor the overall order while independently tracking each requested service.

**Request (REQ)**

The **Request**, commonly abbreviated as **REQ**, represents the overall order submitted by the requester. It acts as the parent record for the entire submission.

Example:

```text
REQ0010001
```

One REQ can contain one or many Requested Items. Its primary purpose is to:

- Track the complete order.
- Maintain overall request status.
- Store requester information.
- Group multiple Requested Items together.

Think of the REQ as the order number generated after placing an online shopping order.

**Requested Item (RITM)**

Each individual Catalog Item inside the Request becomes its own **Requested Item**, commonly known as **RITM**.

For example:

```text
REQ0010001
│
├── RITM0010001 → Laptop Request
├── RITM0010002 → VPN Access
└── RITM0010003 → Microsoft Office
```

Although all three services belong to the same Request, each Requested Item progresses independently. For instance:

- Laptop delivery may take three days.
- VPN access may be completed within one hour.
- Microsoft Office installation may finish the same day.

This independent tracking provides greater flexibility during fulfillment.

### Step 4 — Catalog Tasks (SCTASK)

Each Requested Item may generate one or more **Catalog Tasks (SCTASK)**. These tasks represent the actual work assigned to support teams.

For example, fulfilling a Laptop Request may involve several separate activities.

```text
Laptop Request
      │
      ├── Manager Approval
      ├── Allocate Laptop
      ├── Install Software
      ├── Perform Quality Check
      └── Deliver Device
```

Each Catalog Task can be assigned to a different support group.

Examples:

- Approval → Reporting Manager
- Asset Allocation → Asset Management Team
- Software Installation → IT Support
- Delivery → Logistics Team

This division of work ensures that every team is responsible only for its assigned task.

### Complete Request Hierarchy

The complete record hierarchy can be visualized as:

```text
REQ0010001
│
├── RITM0010001 (Laptop)
│      ├── SCTASK
│      ├── SCTASK
│      └── SCTASK
│
├── RITM0010002 (VPN)
│      ├── SCTASK
│      └── SCTASK
│
└── RITM0010003 (Microsoft Office)
       ├── SCTASK
       └── SCTASK
```

This hierarchy is one of the most important concepts in ServiceNow because it allows administrators and support teams to monitor every stage of fulfillment.

---

## Major Components of the Service Catalog

Apart from understanding the request lifecycle, today's session also introduced the major building blocks that make the Service Catalog work.

### Catalog Items

Catalog Items are the foundation of the Service Catalog. Every service, product, or request that users submit is represented by a Catalog Item.

Examples include:

- Hardware Requests
- Software Installation
- Business Applications
- IT Support
- HR Services
- Facilities Requests

Once submitted, Catalog Items usually generate:

- Request (REQ)
- Requested Item (RITM)

Depending on their configuration, they may also trigger workflows, flows, approvals, notifications, and fulfillment tasks.

### Variables

Variables are the questions presented to users while ordering a Catalog Item. They collect all the information required to fulfill the request accurately.

Examples include:

- Employee Name
- Department
- Laptop Model
- Operating System
- Business Justification
- Delivery Location
- Required Date

Variables help ensure that support teams receive complete information before processing the request.

**Global Variables**

During the session, it was explained that Variables are **Global by default**. This means the same Variable can be reused across multiple Catalog Items instead of creating duplicate questions every time.

For example, the **Employee ID** Variable can be shared between:

- Laptop Request
- Mobile Request
- VPN Access
- Software Installation

This improves consistency and reduces configuration effort.

**Question Choices**

Variables can also contain predefined **Question Choices**.

For example:

```text
Laptop Model
○ Dell Latitude
○ HP EliteBook
○ Lenovo ThinkPad
```

These predefined options help standardize user input. Question Choices can also:

- Restrict available options.
- Control pricing.
- Influence workflow execution.
- Trigger different approvals.

### Record Producers

A **Record Producer** is a special type of Catalog Item. Unlike a standard Catalog Item, a Record Producer creates records **directly inside a ServiceNow table** instead of generating a Request (REQ) and Requested Item (RITM).

Common examples include creating:

- Incident
- Problem
- HR Case
- Change Request

The user only sees a simple form. After submission, ServiceNow automatically creates the corresponding backend record. This provides a much simpler experience for end users.

### Flows

Flows automate the entire fulfillment process behind the scenes. Instead of manually assigning work or sending notifications, Flow Designer executes the business logic automatically.

Flows can perform tasks such as:

- Manager Approval
- Task Creation
- Notifications
- Assignment
- Escalation
- Status Updates
- Fulfillment

This allows organizations to automate repetitive processes with minimal manual intervention.

### Real-Life Example

Imagine a new employee joins an organization. The employee opens the Service Catalog and requests:

- Laptop
- Microsoft Office
- VPN Access

Internally, ServiceNow performs the following sequence:

```text
Catalog Item
      ↓
Shopping Cart
      ↓
REQ0010001
      │
      ├── RITM0010001 (Laptop)
      │      ├── Approval
      │      ├── Asset Allocation
      │      └── Delivery
      │
      ├── RITM0010002 (Microsoft Office)
      │      └── Software Installation
      │
      └── RITM0010003 (VPN)
             └── Network Configuration
```

Although the employee submitted a single order, ServiceNow automatically separates each requested service and routes it to the appropriate teams. This enables parallel processing and faster fulfillment.

### Memory Tip

Think about ordering products from an online shopping website.

```text
Amazon
Product
      ↓
Shopping Cart
      ↓
Order
      ↓
Individual Products
      ↓
Warehouse Tasks
      ↓
Delivery
```

ServiceNow follows almost the same concept.

```text
Service Catalog
Catalog Item
      ↓
Shopping Cart
      ↓
REQ
      ↓
RITM
      ↓
SCTASK
      ↓
Fulfillment
```

The only difference is that instead of delivering physical products, ServiceNow delivers enterprise services.

---

## Creating a Catalog Item

After creating the **Zebronics Catalog** and the **Hardware** category, the next step was to create an actual Catalog Item.

Navigation followed:

```text
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Items
```

From **Maintain Items**, we clicked **New** to create a new Catalog Item. During the demonstration, the following fields were configured.

| Field | Purpose |
|---------|----------|
| Item Name | Name displayed to users inside the Service Catalog |
| Catalog | Defines which Service Catalog the item belongs to |
| Category | Places the item inside an appropriate category |
| Short Description | Brief summary displayed in catalog listings |
| Description | Detailed explanation of the item |
| Meta | Search keywords used to improve discoverability |

### Item Name

The **Item Name** is the primary name displayed to users. In the practical demonstration, a keyboard-related Catalog Item was created. Users will see this name while browsing the Service Catalog. Choosing meaningful names helps users quickly identify the service they need.

### Catalog

The **Catalog** field determines where the Catalog Item will be published. During the lab, the Catalog selected was:

```text
Zebronics Catalog
```

One important point demonstrated during the session was the **Lock** icon beside the Catalog field. After selecting the Catalog, the instructor recommended clicking the **Lock** icon. Locking the field prevents accidental changes while configuring the remaining details of the Catalog Item. This is a small but useful practice that helps avoid configuration mistakes.

### Category

Next, the Catalog Item was placed inside the previously created **Hardware** category.

```text
Catalog
└── Zebronics Catalog
Category
└── Hardware
```

Unlike the Catalog field, the Category field does **not** provide a Lock option. Once selected, the item becomes part of that category and will appear there in the Service Catalog.

### Short Description

The **Short Description** provides a concise overview of the Catalog Item.

Example:

```text
Mechanical USB Keyboard
```

This description is typically displayed in catalog listings and search results, allowing users to quickly understand what the item offers without opening it.

### Description

The **Description** field contains detailed information about the Catalog Item.

Example:

> High-quality mechanical keyboard with USB connectivity, multimedia keys, and an ergonomic design suitable for office and professional use.

Providing detailed descriptions helps users understand exactly what they are requesting before submitting an order.

### Meta

One field that stood out during the demonstration was the **Meta** field. The Meta field stores searchable keywords associated with the Catalog Item.

Although these keywords are not prominently displayed to users, ServiceNow uses them to improve search results inside the Service Catalog.

For example:

```text
Keyboard
Mechanical
USB
VIP
Office
Computer
Peripheral
```

If a user searches for **USB**, **Office**, or **Keyboard**, ServiceNow can use these keywords to suggest the correct Catalog Item. This makes the Meta field extremely useful for improving the search experience.

Think of the Meta field as **Search Engine Optimization (SEO)** for the Service Catalog. Just as websites use keywords to appear in Google search results, Catalog Items use Meta keywords to become easier to discover within ServiceNow. The better the keywords, the easier it is for users to find the correct service.

---

## Process Engine

Another important configuration discussed during the session was the **Process Engine**. The Process Engine determines **how the Catalog Item is fulfilled after a user submits a request**. Rather than simply storing the request, ServiceNow needs to know what automation should execute behind the scenes.

Three options were discussed.

| Process Engine | Purpose |
|----------------|---------|
| **Flow** | Uses Flow Designer (low-code/no-code) for modern automation. |
| **Workflow** | Uses the classic Workflow Editor to automate request processing. |
| **Execution Plan** | Executes a predefined sequence of fulfillment activities. |

**Flow** uses **Flow Designer**, which is ServiceNow's modern automation platform. It provides a low-code/no-code approach for creating automated business processes. Typical Flow activities include Manager Approval, Task Creation, Notifications, Assignments, and Status Updates.

**Workflow** uses the traditional **Workflow Editor**. Although many organizations now prefer Flow Designer, Workflow is still commonly found in existing ServiceNow implementations. It automates the complete lifecycle of the request using predefined workflow activities.

**Execution Plan** performs a predefined sequence of fulfillment tasks. Instead of using graphical workflows, it executes activities in a fixed order. Execution Plans are useful when fulfillment simply follows a predefined checklist.

---

## Picture, Pricing, and Portal Settings

The **Picture** tab allows administrators to upload an image representing the Catalog Item. Examples include a Keyboard Image, Laptop Image, Printer Image, or Monitor Image. Adding images improves the appearance of the Service Catalog and helps users identify items more quickly, creating a much more user-friendly experience compared to displaying only text.

The **Pricing** section defines the financial details associated with a Catalog Item. The fields demonstrated included:

- **Price** – One-time purchase cost.
- **Recurring Price** – Charges that repeat periodically.
- **Recurring Price Frequency** – Defines whether recurring charges occur monthly, yearly, etc.

During the session, it was noted that pricing is displayed in **US Dollars (USD)** by default. These fields are particularly useful in organizations that charge departments for hardware, software licenses, or recurring services.

The **Portal Settings** section controls how the Catalog Item is presented inside the Service Portal. It determines how users view and interact with the item after it is published. Proper Portal Settings improve usability and provide a better overall browsing experience for end users.

---

## Record Producers

A **Record Producer** is a special type of Catalog Item that presents users with a simplified form. Unlike a normal Catalog Item, a Record Producer does **not** create a Request (REQ) and Requested Item (RITM). Instead, it directly creates or updates records inside a ServiceNow table.

Common examples include:

- Incident
- Problem
- HR Case
- Change Request

The user simply fills out a straightforward form, while ServiceNow automatically creates the corresponding backend record. This simplifies the user experience and reduces unnecessary complexity.

---

## Order Guides

An **Order Guide** helps users request multiple related services through a single guided process. Instead of ordering each Catalog Item individually, users answer a few questions, and ServiceNow automatically includes all the relevant items.

For example, a **New Employee Onboarding** Order Guide may automatically include:

- Laptop Request
- Microsoft Office
- VPN Access
- Employee ID Card
- Building Access
- Email Account

This saves time and ensures that nothing important is missed during complex service requests.

**Service Request vs Order Guide**

| Service Request | Order Guide |
|-----------------|-------------|
| Requests a single Catalog Item. | Requests multiple related Catalog Items together. |
| Suitable for simple requests. | Suitable for bundled or guided requests. |
| User selects one service. | ServiceNow groups multiple services automatically. |

---

## Service Catalog Request Output

Every Catalog request follows a consistent hierarchical record structure.

```text
REQ (Overall Request)
│
├── RITM (Requested Item)
│      ├── SCTASK
│      ├── SCTASK
│      └── SCTASK
│
├── RITM (Requested Item)
│      ├── SCTASK
│      └── SCTASK
│
└── RITM (Requested Item)
       └── SCTASK
```

**REQ (Request)** represents the complete order, stores overall request information, and acts as the parent record.

**RITM (Requested Item)** represents each individual Catalog Item and tracks its own fulfillment status.

**SCTASK (Catalog Task)** represents the actual work assigned to support teams. Multiple Catalog Tasks may exist for a single Requested Item.

Understanding this hierarchy is essential because almost every Service Catalog request follows this structure internally.

---

## Catalog Security

Not every Catalog Item should be visible to every user. ServiceNow uses **User Criteria** to determine which users can access specific Catalog Items or Categories.

User Criteria evaluates conditions such as:

- Roles
- Groups
- Departments
- Companies
- Locations

This ensures that users only see services they are authorized to request.

The **Available For** section specifies who is allowed to access the Catalog Item, such as IT Employees, Managers, or the HR Team. The **Not Available For** section specifies who should be restricted, such as Contractors, Temporary Employees, or External Users.

A single Catalog Item can contain multiple User Criteria records, enabling fine-grained access control.

Example:

```text
Laptop Request
Available For:
✓ IT Employees
✓ Managers
Not Available For:
✗ Contractors
```

Only eligible users will be able to view and request the Catalog Item.

---

## Catalog Builder

The final concept introduced was the **Catalog Builder**. Catalog Builder provides a structured, role-based approach for creating, configuring, reviewing, and publishing Catalog Items. Instead of manually navigating multiple modules, administrators are guided through a standardized creation process.

The overall workflow looks like this:

```text
Template
      ↓
Requirements
      ↓
Create Catalog Item
      ↓
Configure Variables
      ↓
Assign Flow
      ↓
Configure User Criteria
      ↓
Review
      ↓
Publish
```

This structured approach helps maintain consistency across large organizations and reduces configuration errors.

### Memory Tip

Think of building an online shopping website.

```text
Product
        ↓
Catalog Item
Product Questions
        ↓
Variables
Quick Contact Form
        ↓
Record Producer
Combo Offer
        ↓
Order Guide
Customer Permissions
        ↓
User Criteria
Product Creation Wizard
        ↓
Catalog Builder
```

Every component has a specific responsibility, and together they create a complete, organized, secure, and user-friendly Service Catalog.

---

## Publishing the Catalog to the Service Portal

After creating the **Zebronics Catalog**, its **Hardware** category, and a **Catalog Item**, the next step was to make the catalog available to end users through the **Service Portal**.

Creating a Catalog inside ServiceNow does **not** automatically make it visible to users. A Catalog must first be associated with a Service Portal before users can browse or request its services.

### Opening the Service Portal

The first step was to access the Service Portal directly. Instead of navigating through the Application Navigator every time, the instructor demonstrated a much quicker method.

Simply copy the instance URL up to the domain name.

Example:

```text
https://<instance>.service-now.com/
```

Open a new browser tab and append **`sp`** to the end of the URL.

```text
https://<instance>.service-now.com/sp
```

This opens the **Service Portal**, which serves as the end-user interface for ServiceNow. Through this portal, users can:

- Browse Service Catalogs
- Submit Service Requests
- View Knowledge Articles
- Track Request Status
- Raise Incidents
- Access Record Producers

At this stage, the newly created **Zebronics Catalog** was **not visible**, because it had not yet been associated with the Service Portal.

### Associating the Catalog with the Service Portal

To make the Catalog available, the following navigation was performed.

```text
All
└── Service Portal
    └── Portals
```

Inside the instance, multiple portals were available, including the Knowledge Portal and the Service Portal. The **Service Portal** record was opened.

After opening the Service Portal configuration, scrolling down revealed the **Catalogs** related list. Initially, only the default catalogs were available. To include the newly created Catalog, there were two options: **New** or **Edit**. During the practical session, **Edit** was used.

Inside the Edit window:

1. Locate **Zebronics Catalog**.
2. Move it to the selected list.
3. Save the configuration.

This associates the newly created Catalog with the Service Portal.

### Verifying the Configuration

After saving the changes, the Service Portal was refreshed.

```text
https://<instance>.service-now.com/sp
```

This time, the **Zebronics Catalog** appeared successfully on the homepage. The Catalog was now available for users to browse and place requests. This confirmed that the publishing process had been completed successfully.

### Why is this Step Required?

One important concept highlighted during the session was that **creating a Catalog and publishing a Catalog are two separate processes**. Creating a Catalog only stores it inside the ServiceNow instance. Publishing determines **where users can actually access it**.

This allows organizations to:

- Publish different catalogs to different portals.
- Hide internal catalogs from external users.
- Maintain separate portals for IT, HR, Facilities, or Customer Service.
- Control the overall user experience.

Without associating the Catalog with a Service Portal, users cannot access it, even though it already exists within ServiceNow.

### Memory Tip

Think of creating an online shopping website.

```text
Create Product Collection
          ↓
Add Categories
          ↓
Add Products
          ↓
Publish Collection
          ↓
Customers Can Browse
```

Similarly, in ServiceNow:

```text
Create Catalog
        ↓
Create Category
        ↓
Create Catalog Item
        ↓
Associate with Service Portal
        ↓
Refresh /sp
        ↓
Users Can Browse
```

Simply creating the Catalog is not enough—it must also be published to the appropriate portal.

---

## Testing the Record Producer

The final practical activity demonstrated how a **Record Producer** works inside the Service Portal. After completing the Record Producer configuration and mapping the required Variable to the target field, the Record Producer was saved. The next step was to verify whether it actually created records inside the **Problem** table.

### Opening the Record Producer

Inside the Service Portal, the following navigation was performed:

```text
Service Portal
        ↓
Get Help
        ↓
Can We Help You?
        ↓
Create Problem Table
```

Opening the Record Producer displayed a simplified form. Unlike standard forms inside the platform, the Record Producer only displayed the fields required from the end user. The form contained a **Problem Statement** field and an **Add Attachments** option. This simplified interface makes it much easier for users to submit requests without needing to understand the underlying database structure.

### Submitting a Problem

A sample Problem Statement was entered.

Example:

> Multiple users are unable to access the company VPN after the latest system update. The issue affects employees across different departments and has been reported by several users.

Other examples discussed included:

- Email notifications are not being delivered after a recent upgrade.
- Employees are experiencing slow system performance.
- Printer services are unavailable after network maintenance.
- Users cannot log in to the HR Portal despite entering valid credentials.

After entering the details, the **Submit** button was clicked.

### What Happens Behind the Scenes?

Although the user only sees a simple form, ServiceNow performs several operations automatically. After clicking **Submit**, ServiceNow:

- Creates a new record in the **Problem** table.
- Generates a unique **Problem Number**.
- Stores the mapped field values.
- Displays the generated Problem Number as confirmation.

For example:

```text
PRB0010001
```

This confirms that the Record Producer successfully created a backend record.

### Verifying the Problem Record

To confirm that everything worked correctly, the Developer Instance was opened.

Navigation:

```text
All
└── Problem
    └── All
```

To locate the newly created record, a filter was applied.

```text
Created
        ↓
Today
```

After applying the filter, the newly created Problem record appeared in the list. This confirmed that the Record Producer had successfully inserted the record into the Problem table.

### Viewing the Record

Opening the Problem record allowed verification of the stored information. The following fields were reviewed:

- Problem Number
- Problem Statement
- Created By
- Created Date
- Description (if mapped)

One important observation made during the demonstration was the **Created By** field. When the Record Producer was submitted while logged in as **System Administrator**, the record correctly displayed:

```text
Created By
System Administrator
```

This confirmed that the record retained the correct user information.

### Creating Another Record

To further verify the functionality, another Problem was submitted through the same Record Producer. ServiceNow again generated a new Problem Number and inserted another record into the Problem table. After refreshing the Problem list, both records were visible. This demonstrated that every submission generates a completely new record rather than modifying an existing one.

---

## Interview Notes

The following are some important interview questions and concepts that I revised after completing today's session.

**What is a Service Catalog?**
A **Service Catalog** is a centralized self-service portal in ServiceNow that allows users to browse, request, and order enterprise services, hardware, software, and support through standardized Catalog Items. Once a request is submitted, ServiceNow automatically manages approvals, task assignments, notifications, and fulfillment using Flows or Workflows.

**What is a Catalog Item?**
A **Catalog Item** is an individual service or product available within the Service Catalog. Examples include Laptop Request, Software Installation, VPN Access, and Employee ID Card. Each Catalog Item collects user input using Variables and generates records such as Requests (REQ) and Requested Items (RITM) after submission.

**What is the difference between a Catalog and a Category?**

| Catalog | Category |
|----------|----------|
| Highest level container for services. | Organizes Catalog Items inside a Catalog. |
| Can contain multiple Categories. | Belongs to one Catalog. |
| Example: Zebronics Catalog | Example: Hardware |

A Catalog is similar to a shopping website, while Categories are like departments inside that website.

**What is the difference between REQ, RITM, and SCTASK?**

| Record | Purpose |
|---------|---------|
| **REQ** | Represents the complete service request submitted by the user. |
| **RITM** | Represents each individual Catalog Item within the Request. |
| **SCTASK** | Represents the fulfillment tasks assigned to support teams. |

**What is a Record Producer?**
A **Record Producer** is a special type of Catalog Item that presents a simplified form to users and creates records directly inside a ServiceNow table instead of generating REQ and RITM records. Examples include Incident, Problem, HR Case, and Change Request.

**What are Variables?**
Variables are the questions displayed to users while ordering a Catalog Item, such as Employee Name, Department, Laptop Model, and Required Date. Variables collect all the information required for successful request fulfillment.

**What is the Meta field used for?**
The **Meta** field stores searchable keywords related to a Catalog Item. These keywords improve search results inside the Service Catalog. A simple way to remember it is that Meta is like SEO for the Service Catalog.

**What are the Process Engine options?**

| Process Engine | Description |
|----------------|-------------|
| **Flow** | Modern low-code/no-code automation using Flow Designer. |
| **Workflow** | Classic Workflow Editor used in older implementations. |
| **Execution Plan** | Executes a predefined sequence of fulfillment activities. |

**Why are Categories important?**
Categories improve navigation by grouping related Catalog Items together. Instead of searching through hundreds of services, users simply browse the appropriate category.

**Why do we publish a Catalog to the Service Portal?**
Creating a Catalog inside ServiceNow does **not** automatically make it visible to users. To make it available, the Catalog must be associated with a **Service Portal**. Only then can users browse and request its Catalog Items.

---

## Best Practices Learned

During today's session, a few practical tips stood out that are useful while configuring the Service Catalog:

- **Use meaningful Catalog names** that clearly represent the department or business function, such as IT Catalog, HR Catalog, or Facilities Catalog.
- **Organize Catalog Items into Categories** instead of placing every item in one location.
- **Write clear descriptions** — a well-written Short Description and Description reduce confusion and improve the overall user experience.
- **Use Meta keywords wisely** to make Catalog Items easier to discover through search.
- **Lock the Catalog field** while creating a Catalog Item to prevent accidental changes during configuration.
- **Upload images whenever possible** to make the Service Catalog look cleaner and help users quickly recognize products and services.
- **Reuse Variables** instead of creating duplicate questions repeatedly, keeping the Service Catalog consistent and easier to maintain.
- **Secure Catalog Items** using **User Criteria** to ensure users only see the services they are authorized to request.

### Common Mistakes to Avoid

- Creating Catalog Items without assigning Categories.
- Forgetting to publish the Catalog to the Service Portal.
- Using poor descriptions.
- Ignoring Meta keywords.
- Creating duplicate Variables unnecessarily.
- Forgetting to configure the fulfillment process.
- Not testing the Catalog Item after publishing.

---

## Memory Map

The complete Service Catalog workflow can be remembered using the following diagram.

```text
Service Catalog
        │
        ├── Catalog
        │       │
        │       ├── Categories
        │       │       │
        │       │       ├── Catalog Items
        │       │       │       │
        │       │       │       ├── Variables
        │       │       │       ├── Process Engine
        │       │       │       ├── Pricing
        │       │       │       ├── Picture
        │       │       │       └── Portal Settings
        │
        ├── Publish to Service Portal
        │
        ├── User Requests Item
        │
        ├── Shopping Cart
        │
        ├── REQ
        │
        ├── RITM
        │
        ├── SCTASK
        │
        └── Fulfillment
```

### Quick Revision Sheet

```text
Maintain Catalogs
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Catalogs
```

```text
Maintain Categories
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Categories
```

```text
Maintain Items
All
└── Service Catalog
    └── Catalog Definitions
        └── Maintain Items
```

```text
Service Portal
https://<instance>.service-now.com/sp
```

```text
Portal Configuration
All
└── Service Portal
    └── Portals
```

```text
Problem Records
All
└── Problem
    └── All
```

---

## Key Takeaways

- Understood the purpose and architecture of the Service Catalog.
- Learned the complete request lifecycle from **Catalog Item → REQ → RITM → SCTASK**.
- Explored the major components of the Service Catalog, including Catalog Items, Variables, Record Producers, Order Guides, and User Criteria.
- Performed hands-on creation of a custom **Zebronics Catalog** and **Hardware** category.
- Created and configured a Catalog Item with descriptions, metadata, pricing, and process engine settings.
- Learned the importance of the **Meta** field in improving Catalog Item searchability.
- Understood the differences between **Flow**, **Workflow**, and **Execution Plan**.
- Published a Catalog to the Service Portal and verified its visibility.
- Tested a Record Producer by creating Problem records directly from the Service Portal.
- Verified backend record creation in the **Problem** table using filters and confirmed successful record generation.

---

## Reflection

Today's session was one of the most practical and insightful sessions of the internship so far because it connected multiple ServiceNow concepts into a complete business workflow.

I started by understanding the theory behind the Service Catalog and why organizations use it as a centralized self-service portal. From there, I explored the complete request lifecycle, learned how Catalog Items, Variables, Record Producers, and User Criteria work together, and understood how ServiceNow manages requests from submission to fulfillment.

The hands-on exercises reinforced these concepts even further. Creating the **Zebronics Catalog**, organizing it with the **Hardware** category, configuring a **Catalog Item**, publishing it to the **Service Portal**, and finally testing a **Record Producer** showed how administrators transform business requirements into services that end users can request with just a few clicks.

One of the biggest takeaways from today's session was realizing that ServiceNow is much more than a ticketing platform. Every request submitted by a user triggers a structured series of automated actions—from record creation and approvals to task assignments and fulfillment. This ability to standardize and automate business processes is what makes the Service Catalog such a powerful component of the ServiceNow platform.

Overall, today's learning strengthened my understanding of how enterprise service requests are designed, organized, secured, published, and fulfilled within ServiceNow. It also gave me practical experience that I can build upon in future modules involving Flows, Workflows, Request Fulfillment, and Service Portal customization.

This repository will serve as my daily learning journal throughout the AICTE × ServiceNow Virtual Internship Program, documenting both theoretical concepts and practical exercises as I continue exploring the ServiceNow ecosystem.
