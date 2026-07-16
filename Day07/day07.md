# Day 07 - Flow Designer Fundamentals & Process Automation

**Date:** 16 July 2026

---

## Introduction

Today's session shifted focus from the Service Catalog covered in Day 06 towards one of the most powerful automation capabilities in ServiceNow—**Flow Designer**. While Day 06 explored how end users request services through a centralized portal, Day 07 explored what happens behind the scenes once a request or record is created: how ServiceNow automates approvals, updates, notifications, and task assignments without writing large amounts of code.

The session began with the theory behind Flow Designer, its purpose, and its core components—Triggers, Conditions, Actions, and Data Pills. This was followed by a complete hands-on exercise in which an Incident automation flow was built, activated, and tested. Later, the session covered the Flow Diagramming View, Application Scopes, and one of the most important administrative concepts in ServiceNow—Update Sets—along with a practical exercise creating and completing an Update Set. The day concluded with an overview of Integration Hub, Spokes, and Process Automation Designer, along with a look at Variables and Variable Sets, which tie directly back into how Catalog Items and Flows collect and reuse information.

---

## What is Flow Designer?

**Flow Designer** is a no-code/low-code process automation tool in ServiceNow that provides a visual interface to create, manage, and automate business processes. It allows users to build workflows using a drag-and-drop interface instead of writing complex scripts.

Flow Designer helps automate business logic by connecting Triggers, Conditions, Actions, and Data into a complete automated workflow.

> **Interview Definition:**
> **Flow Designer** is a no-code/low-code process automation tool in ServiceNow that provides a visual interface for creating, managing, and automating business processes using triggers, conditions, actions, and logic.

### Purpose of Flow Designer

The purpose of Flow Designer can be summarized as: *"Streamline business processes with intuitive visual workflow automation designed for operations teams and product managers."*

**Streamline Business Processes** — Flow Designer helps organizations make business tasks faster, simpler, more efficient, and less dependent on manual work. For example, instead of manually approving every laptop request, Flow Designer can automatically send approval requests, create IT tasks, notify users, and update records.

**Intuitive Visual Workflow Automation** — Flow Designer provides drag-and-drop workflow building, visual process design, and configuration-based automation, allowing users to create workflows without writing large amounts of code.

**Designed for Operations Teams and Product Managers** — Flow Designer is not only for developers. It allows developers, administrators, operations teams, product managers, and citizen developers to automate processes easily.

### Why Was Flow Designer Introduced?

Flow Designer was introduced to replace traditional workflow approaches with a modern automation platform. Benefits include faster development, easier maintenance, low-code automation, and better collaboration between technical and business users.

### Navigation to Flow Designer

```text
All
└── Process Automation
    └── Flow Designer
```

This opens the Flow Designer workspace where users can create and manage flows.

### What Are Flows?

A **Flow** automates business logic for applications and processes. Flows can automate approvals, task creation, email notifications, record operations, business processes, and external integrations.

**Real-World Example — Laptop Request Automation**

Without automation, an employee submits a laptop request, waits for a manager to manually approve it, waits again for IT to manually create tasks, and receives updates manually. Using Flow Designer, the same process becomes:

```text
Employee submits Laptop Request
              ↓
Flow Trigger Starts
              ↓
Send Manager Approval
              ↓
Create IT Task
              ↓
Send Notification
              ↓
Close Request
```

All of these steps are created visually without complex scripting.

---

## Flow Components

A Flow consists of three main components—**Trigger**, **Conditions**, and **Actions**—with additional data handling done using **Data Pills**.

### Triggers

A trigger defines *when a flow should start executing*. It is the starting point of a flow.

```text
New Incident Created
        ↓
Flow Starts
```

**Types of Triggers**

| Trigger Type | Description | Example |
|---------------|-------------|---------|
| Record-Based | Flow starts when a record is created, updated, or deleted | Incident Created → Flow Executes |
| Date-Based | Flow runs at a specific date/time or schedule | Every Monday 9 AM → Send Pending Approval Reminder |
| Application-Based | Flow starts when application-specific conditions are met | New Employee Created in HR → Start Onboarding Flow |

### Conditions

Conditions add decision-making logic to flows, allowing workflows to create branching paths based on specific criteria.

```text
Incident Priority = High?
```

If TRUE → Notify Manager, Create Escalation Task. If FALSE → Continue Normal Process.

**Condition Operators**

| Operator | Checks | Example Condition | Matches | Does Not Match |
|-----------|--------|--------------------|---------|-----------------|
| Contains | Value exists anywhere in the text | Short Description contains WIFI | WIFI, WIFI is not working, My WIFI is down | — |
| Starts With | Text begins with the given value | Short Description starts with WIFI | WIFI issue, WIFI not working | My WIFI is not working |
| Ends With | Text ends with the given value | Short Description ends with WIFI | Problem with WIFI | WIFI is not working |

### Actions

Actions are operations executed by the system after a trigger or condition is satisfied. They perform the actual work in a flow, such as looking up records, updating records, creating records, requesting approvals, sending notifications, and logging values.

```text
Update Incident Record
Category = Software
```

### Data Pills

**Data Pills** are reusable pieces of information generated by triggers and actions. They allow data to move between different steps of a flow. For example, an Incident trigger creates Incident Number, Caller, Priority, Assignment Group, and Short Description, which can then be used in later actions.

```text
Add Action
      ↓
Data Pill Created
      ↓
Reference Data
      ↓
Use in Actions
```

The basic flow structure ties all of this together:

```text
Trigger
   ↓
Condition
   ↓
Action
   ↓
Output
```

---

## Hands-on: Building an Incident Automation Flow

**Scenario:** Automatically update Incident details based on the Short Description. When an Incident is created or updated with a Short Description containing "WIFI", automatically update Category → Software, Subcategory → Email, and Assignment Group → Software.

**Step 1: Create a Test Incident**

Navigate to the Incident table and create a new Incident with Caller set to any user and Short Description set to "WIFI". Initially, Category, Subcategory, and Assignment Group are all left blank, then the Incident is submitted.

**Step 2: Create a New Flow**

```text
All
└── Process Automation
    └── Flow Designer
        └── New Flow
```

Configure the Flow Name, set Application to Global, and set Run As to System User, then click **Build**.

**Step 3: Add Trigger**

Click **+ Add Trigger** and select **Record**, since the automation depends on an Incident record. Configure Table as Incident and Trigger Event as **Created or Updated**, so the flow runs both when a new Incident is created and when an existing Incident is modified.

**Step 4: Add Condition**

Click **+ Add Condition** and configure Field = Short Description, Operator = Contains, Value = WIFI.

**Step 5: Add Action**

Click **+ Add Action**, select **ServiceNow Core**, and choose **Update Record**.

**Step 6: Configure Update Record Action**

- Record Input → Data Pill: Trigger → Record (passes the current Incident record into the action)
- Table → Incident
- Update Fields → Category = Software, Subcategory = Email, Assignment Group = Software

**Final Flow Logic**

```text
Incident Created / Updated
          ↓
Condition: Short Description contains WIFI
          ↓
Update Record Action
          ↓
Category = Software
Subcategory = Email
Assignment Group = Software
```

**Step 7: Activate Flow**

Clicking **Activate** validates the flow, and the system confirms: *Flow activated successfully, no errors found.*

**Step 8: Test the Flow**

A new Incident was created with Short Description "WIFI". Before the flow ran, Category, Subcategory, and Assignment Group were all blank. After submitting, the flow automatically updated Category to Software, Subcategory to Email, and Assignment Group to Software—confirming the automation worked as designed.

**Observations**

A flow can only contain **one trigger**, but it can contain **multiple actions**. During testing, it was also observed that conditions depend heavily on the exact string value used in the Short Description, so it's important to verify flow execution details whenever the expected automation does not run.

---

## Flow Diagramming View

**Flow Diagramming View** provides a visual representation of a flow in Flow Designer. It helps users understand flow structure, trigger points, conditions, actions, and execution paths, making complex workflows easier to design and analyze.

**Components of Flow Diagramming View**

- **View Selector** — switches between Flow view and Configuration view.
- **Undo and Redo Controls** — undo recent changes or restore previously undone changes.
- **Search Nodes** — quickly find specific triggers, actions, or flow steps inside large flows.
- **Node Properties** — displays inputs, outputs, settings, and configuration values of the selected node.
- **Zoom Controls** — zoom in or out to navigate large workflow diagrams.
- **Download Diagram** — exports flow diagrams for documentation, presentations, or sharing, supported in **SVG** (best for documentation, scales without quality loss) and **PNG** (best for screenshots and reports) formats.

**Important Note:** Flow Designer automatically disables Diagramming View when a flow contains unsupported components, since some components cannot be represented visually in the diagram.

---

## Application Scopes

**Application Scoping** isolates applications by identifying and restricting access to available artifacts and data. It helps protect custom applications, prevent conflicts between different solutions, and control access between applications.

Without proper isolation, different applications may modify each other's objects, data conflicts may occur, and customizations may become difficult to manage. Application Scoping provides better security and organization.

**Types of Application Scopes**

| Scope | Characteristics |
|--------|------------------|
| Global | Default ServiceNow scope; shared platform scope accessible across applications; used for core ServiceNow functionality |
| Private | Used for custom applications; restricts access to application-specific artifacts; protects custom tables, scripts, and objects |
| Isolation | Controls communication between applications; prevents unauthorized access, conflicts, and data exposure |

---

## Update Sets

**Update Sets** are the primary mechanism used to capture and move customizations between ServiceNow instances. They package configuration changes into deployable units, allowing administrators to group a series of configuration changes into a named collection and move them as a complete unit.

Common movement path:

```text
Development
      ↓
Testing
      ↓
Production
```

**What Can Update Sets Capture?**

Update Sets can capture configuration changes, scripts, UI Actions, Business Rules, Client Scripts, Dictionary changes, tables, fields, forms, and other configuration artifacts. They are stored in the `sys_update_set` table.

**Update Set Lifecycle**

```text
Create Update Set
        ↓
Capture Changes
        ↓
Complete Update Set
        ↓
Export
        ↓
Import
        ↓
Preview
        ↓
Commit
```

**Types of Update Sets**

| Type | Description |
|-------|-------------|
| Global Update Set | System-level update set used to capture changes made in the Global application scope |
| Local Update Set | Created in the current instance to capture local configuration changes |
| Retrieved Update Set | Imported from another ServiceNow instance for deployment |

**Backout Capability**

Backout capability allows administrators to revert changes introduced by an update set, restoring the previous configuration state if problems occur after deployment.

### Update Set Best Practices

- **Use descriptive names**, such as `Incident_Automation_Enhancement`, and avoid generic names like `Update Set 1`.
- **Test before promotion**, following a DEV → TEST → PROD flow.
- **Use version control systems** to back up configuration, track changes, and maintain history.
- **Document changes** — what changed, why, who changed it, and when — for auditing, troubleshooting, and recovery.
- **Keep update sets organized** by creating separate update sets for different features, for easier troubleshooting and cleaner deployment.

### Update Set Hands-on

**Step 1: Navigate to Local Update Sets**

```text
All
└── Search Update
    └── Local Update Sets
```

**Step 2 & 3: Create and Configure a New Update Set**

Click **New** and configure Name = `VIP6`, State = In Progress, Application = Global.

**Submit Options**

- **Submit** — saves the update set.
- **Submit and Make Current** — saves the update set and makes it active, so future configuration changes are captured automatically.

**Step 4: Create Configuration Changes**

A custom table was created with Label = `Reading`, Table Name = `u_Reading`, containing two fields:

- **Life** — Type: Reference
- **Earth** — Type: String

**Step 5: Create Records**

Records such as `123` and `2345` were created inside the new table, and these changes were captured automatically by the active update set.

**Step 6: Complete Update Set**

Navigating back to Local Update Sets, opening `VIP6`, and changing State → Complete stops the update set from capturing further changes and removes the red capture indicator.

**Related Links after Completion**

- **Export to XML** — exports the update set as an XML file for transferring to another instance.
- **Scan Update Set** — checks the update set for possible problems before deployment.
- **Merge with Another** — combines changes from another update set into the current one.

---

## Integration Hub

**Integration Hub** is a ServiceNow capability used alongside Flow Designer to connect ServiceNow with external applications. It provides pre-built integration actions to communicate with third-party systems.

The pre-built collections of integration actions are called **Spokes**. A spoke contains reusable actions for a specific application or service.

**Common Integration Hub Examples**

| Example | Used For |
|----------|-----------|
| Email Integration | Sending automated emails and notifications |
| Document Systems | Uploading, retrieving, and managing documents |
| Cloud Services & Calendar Tools | Cloud resource management, calendar scheduling, creating events |

---

## Process Automation Designer (PAD)

**Process Automation Designer** is used to design and manage complex business processes visually, helping organizations build structured workflows across departments.

- **Citizen Developer** — enables non-technical users to create workflows using low-code/no-code tools, reducing dependency on developers and enabling faster, visual automation.
- **Process Definition** — creates complex workflows using **Triggers** (define when the process starts), **Lanes** (represent different users, teams, or departments involved), and **Activities** (represent tasks performed during the process).
- **Cross Enterprise** — allows workflow owners to create processes that work across the entire organization, with multiple departments participating in one workflow.

Example process spanning lanes:

```text
Employee Created
↓
HR Lane
Verify Documents
↓
IT Lane
Create Account
↓
Manager Lane
Assign Tasks
```

---

## Variables and Variable Sets

Since much of today's automation revolves around collecting and reusing data across a process, it's worth connecting Flow Designer back to a concept first introduced in Day 06—**Variables**.

**Variables** are the questions displayed to users while ordering a Catalog Item or submitting a form, such as Employee Name, Department, Laptop Model, or Required Date. Variables collect the information required for successful request fulfillment, and their values can later be referenced in Flows through Data Pills.

**Variable Sets** take this a step further. A **Variable Set** is a reusable, named group of Variables that can be attached to multiple Catalog Items at once, instead of recreating the same set of questions repeatedly on every item.

**Why Variable Sets Matter**

- **Reusability** — the same group of variables (e.g., "Employee Details": Name, Department, Manager) can be added to many Catalog Items in one step.
- **Consistency** — every Catalog Item using the set collects information in the same format, reducing configuration errors.
- **Easier Maintenance** — updating a Variable Set updates it everywhere it is used, instead of editing each Catalog Item individually.

**Types of Variable Sets**

| Type | Description |
|-------|-------------|
| One to One | Attached to a single Catalog Item only |
| One to Many | A single Variable Set shared across multiple Catalog Items |

Together, Variables and Variable Sets form the data-collection layer that feeds into automation—whatever is captured from the user becomes available as Data Pills further down the line in a Flow, connecting Day 06's Service Catalog concepts directly to Day 07's Flow Designer automation.

---

## Interview Questions & FAQs

**How do you create a flow in Flow Designer?**
Create a new flow, select the application scope, configure the trigger, add conditions and actions, test the flow, and activate it.

**Can a flow have multiple triggers?**
No. A Flow Designer flow can have only one trigger, but it can contain multiple actions.

**What is the purpose of the Record Trigger?**
A Record Trigger starts a flow when a record is created, updated, or deleted in ServiceNow.

**What is Update Record Action?**
Update Record Action modifies fields or values of an existing ServiceNow record.

**What are Data Pills used for?**
Data Pills pass information generated by triggers or actions into other steps of a flow.

**What is Integration Hub?**
Integration Hub is a ServiceNow capability that provides pre-built integration actions to connect ServiceNow with external applications.

**What are Spokes in ServiceNow?**
Spokes are collections of pre-built integration actions used to connect ServiceNow with specific external systems.

**What is Process Automation Designer?**
Process Automation Designer is a visual tool used to design and manage complex business workflows across multiple teams and departments.

**What is a Citizen Developer?**
A Citizen Developer is a non-technical user who can create applications and workflows using low-code/no-code tools.

**What is an Update Set in ServiceNow?**
An Update Set is a container used to capture and move configuration changes from one ServiceNow instance to another.

**Why are Update Sets used?**
Update Sets are used to safely migrate customizations between development, testing, and production environments.

**What does an Update Set capture?**
Update Sets capture configuration changes such as scripts, UI actions, business rules, tables, fields, and other customizations.

**What are the types of Update Sets?**
Global Update Set, Local Update Set, and Retrieved Update Set.

**What is a Retrieved Update Set?**
A Retrieved Update Set is an update set imported from another ServiceNow instance for review and deployment.

**What is the purpose of Backout capability?**
Backout capability allows administrators to undo changes introduced by an update set and restore the previous configuration.

**What is Application Scoping?**
Application Scoping is a ServiceNow feature that isolates applications by controlling access to application artifacts and data.

**What is the difference between Global Scope and Private Scope?**
Global Scope contains shared platform objects, while Private Scope restricts access to objects belonging to a specific custom application.

**When is Flow Diagramming View disabled?**
Flow Diagramming View is disabled when a flow contains unsupported components that cannot be represented visually.

**What formats can Flow diagrams be exported in?**
Flow diagrams can be exported as SVG or PNG.

**What is a Variable Set?**
A Variable Set is a reusable, named group of Variables that can be attached to multiple Catalog Items, keeping data collection consistent and easier to maintain.

---

## Best Practices Learned

- Use a single, well-scoped trigger per flow, and add multiple actions rather than building separate overlapping flows.
- Always test a flow before activating it in a production environment.
- Use descriptive, meaningful names for both Flows and Update Sets.
- Keep Update Sets organized by feature rather than bundling unrelated changes together.
- Document configuration changes for auditing and future troubleshooting.
- Reuse Variable Sets across Catalog Items instead of duplicating the same variables repeatedly.
- Verify flow execution details whenever an expected automation does not trigger as intended.

---

## Memory Map

```text
Flow Designer
        │
        ├── Trigger (Record / Date-Based / Application-Based)
        │
        ├── Conditions (Contains / Starts With / Ends With)
        │
        ├── Actions (Update / Create / Notify / Approve)
        │
        ├── Data Pills
        │
        ├── Flow Diagramming View
        │
        ├── Integration Hub
        │       └── Spokes
        │
        ├── Process Automation Designer
        │       ├── Citizen Developer
        │       ├── Process Definition (Triggers, Lanes, Activities)
        │       └── Cross Enterprise
        │
        ├── Application Scopes (Global / Private / Isolation)
        │
        ├── Update Sets (Global / Local / Retrieved)
        │
        └── Variables & Variable Sets
```

### Quick Revision Sheet

```text
Flow Designer
All
└── Process Automation
    └── Flow Designer
```

```text
Local Update Sets
All
└── Search Update
    └── Local Update Sets
```

---

## Key Takeaways

- Understood Flow Designer as ServiceNow's no-code/low-code process automation tool.
- Learned the core Flow components: Triggers, Conditions, Actions, and Data Pills.
- Built, activated, and tested a real Incident automation flow that updates Category, Subcategory, and Assignment Group based on Short Description.
- Learned that a flow can have only one trigger but multiple actions.
- Explored Flow Diagramming View and its export formats (SVG, PNG).
- Understood Application Scoping and the difference between Global, Private, and Isolation scopes.
- Learned the purpose, lifecycle, and types of Update Sets, and performed a hands-on creation and completion of an Update Set.
- Explored Integration Hub, Spokes, and Process Automation Designer for cross-department automation.
- Connected Variables and Variable Sets from Day 06's Service Catalog concepts to how data flows into automation.

---

## Reflection

Day 07 marked a shift from understanding how users request services to understanding how ServiceNow automates what happens after a request or record is created. Flow Designer stood out as one of the most practical tools covered so far, because it turned an abstract idea—automation—into something I could build, activate, and immediately see working on a test Incident.

The hands-on exercise of automatically updating Category, Subcategory, and Assignment Group based on a WIFI-related Short Description made the concepts of Triggers, Conditions, and Actions click in a way that theory alone couldn't. Seeing the "before and after" state of the Incident confirmed just how much manual work automation can eliminate.

Beyond Flow Designer, learning about Update Sets gave me a clearer picture of how ServiceNow administrators safely move configuration changes from development to production without disrupting live environments. Creating, capturing, and completing an actual Update Set made the DEV → TEST → PROD lifecycle feel much more concrete.

Finally, revisiting Variables through the lens of Variable Sets helped tie Day 06 and Day 07 together—showing how the same data a user provides while ordering a Catalog Item can flow directly into an automated process through Data Pills. Overall, today's session strengthened my understanding of how ServiceNow connects data collection, decision logic, and automation into one continuous, low-code system.

This repository will serve as my daily learning journal throughout the AICTE × ServiceNow Virtual Internship Program, documenting both theoretical concepts and practical exercises as I continue exploring the ServiceNow ecosystem.
