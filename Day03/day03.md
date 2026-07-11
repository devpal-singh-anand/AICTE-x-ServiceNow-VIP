# Day 03 — Tables, Schema Map & Access Control Lists (ACLs)
> **Date:** 11 July 2026
>
> Today's session revisited **Tables** in greater depth before moving into two major new topics — the **Schema Map** and **Access Control Lists (ACLs)**. We explored how ServiceNow visualizes relationships between tables, how role-based security is structured, and how administrators create and test ACLs to control access at both the record and field level.

---

# Table of Contents
1. Introduction
2. Part 1 — ServiceNow Tables (Deep Dive)
3. Part 2 — Schema Map
4. Part 3 — Introduction to Access Control Lists (ACLs)
5. Part 4 — Creating and Testing ACLs
6. Navigation Paths Learned
7. Important Terminology
8. Practical Activities Performed
9. Key Takeaways
10. Interview Questions
11. Personal Reflection
12. Conclusion
13. What's Next?

---

# Introduction

Before diving into ServiceNow security, today's session began with a deeper understanding of **Tables** — the foundation of the entire platform. Once the data model was reinforced, the session moved on to the **Schema Map**, a visual tool for understanding how tables relate to one another, and finally into **Access Control Lists (ACLs)**, which govern who can access what within the platform.

By the end of the day, the session connected three ideas into one continuous story: data lives in **tables**, tables are **related** to each other (visualized through the Schema Map), and access to that data is **secured** through ACLs and Role-Based Access Control (RBAC).

---

# Part 1 — ServiceNow Tables (Deep Dive)

## What is a Table?

A **Table** is a database object where records are stored and managed. A table organizes related information into rows and columns, making it easier to store, retrieve, and manage data.

```text
Incident Table

--------------------------------------------------------
| Number | Caller | State | Priority | Assigned To |
--------------------------------------------------------
| INC001 | Beth   | New   | High     | David       |
| INC002 | Mark   | Open  | Medium   | Sarah       |
--------------------------------------------------------
```

## Records and Fields Recap

- A **Record** represents one individual row inside a table (e.g., `INC0010001`).
- A **Field** represents one column inside a table (e.g., Number, Caller, State, Priority).

## Common Field Types

- String
- Integer
- Choice
- Reference
- Boolean
- Date
- Date/Time
- Email
- Phone
- URL

## Default Tables in ServiceNow

ServiceNow comes with more than **6,000 Out-of-the-Box (OOB) tables**, including Incident, Problem, Change Request, User, Group, CMDB, Knowledge Base, Asset, and Service Catalog. These tables are provided by ServiceNow and do not need to be created manually.

## Custom Tables

Organizations often have business-specific requirements not covered by default tables. Examples include VIP Cohort, Team Celebration, Employee Awards, Visitor Management, and Event Registration.

## Default System Fields

Whenever a new custom table is created, ServiceNow automatically generates **six default system fields**. These remain empty until records are inserted.

## Core Tables vs Custom Tables

| Feature | Core Table | Custom Table |
|----------|------------|----------------|
| Created by | ServiceNow (OOB) | Administrator/Developer |
| Deletable | No | Yes (with confirmation) |
| Scope | Platform-wide | Organization-specific |
| Examples | Task, Incident, Problem, Change Request, User | Team Celebration, VIP Cohort |

## Deleting Tables

- **Core Tables** cannot normally be deleted — the Incident table, for example, does not display a Delete option under **Actions on Selected Rows**.
- **Custom Tables** can be deleted. ServiceNow requires the administrator to type `delete` to confirm, preventing accidental removal.

### Best Practice Before Deleting a Table
- Delete all records before deleting the table.
- Ensure no applications reference the table.
- Verify the table is no longer required.

## Table Hierarchy: Base, Parent & Child

```text
Task  (Base / Parent Table)
│
├── Incident
├── Problem
└── Change Request
```

- **Base Table** — the foundation of a hierarchy (e.g., Task).
- **Parent Table** — any table another table inherits from.
- **Child Table** — inherits fields and behavior from its parent while adding its own fields.

## Table Relationships

| Relationship | Description | Example |
|---------------|-------------|---------|
| One-to-Many (1:N) | One record relates to many records elsewhere | One Department → Many Employees |
| Many-to-Many (M:N) | Many records relate to many records, via a junction table | Students ↔ Courses |
| Database View | Combines data from multiple tables into a virtual view | Cross-table reporting |
| Table Extension | One table inherits another's structure | Task → Incident |

---

# Part 2 — Schema Map

## What is a Schema Map?

A **Schema Map** is a graphical visualization tool that displays how a selected table relates to other tables within the platform — helping administrators understand inheritance, references, and overall database structure without manually inspecting each table.

## Components of a Schema Map

| Component | Description |
|------------|--------------|
| Table Node | Rectangular box representing one table (Label, Name, Type) |
| Relationship Connector | Line showing how tables relate (Extension or Reference) |
| Filter Controls | Toggle which relationship types are displayed |
| Navigation Controls | Zoom In/Out, Pan, Center View |

## Extension Relationships (Inheritance)

```text
Task
│
├── Incident
├── Problem
└── Change Request
```

## Reference Relationships

```text
Incident
     │
     ▼
Caller
     │
     ▼
User
```

The Incident table does not inherit from User — it **references** a record inside the User table.

## Required Roles to Access Schema Map

| Role | Purpose |
|------|---------|
| `admin` | Full administrative access, including Schema Maps |
| `personalize_dictionary` | Access to Dictionary features and Schema Maps |

## Role-Based Access Control (RBAC) Model

```text
User
   │
   ▼
Group
   │
   ▼
Role
   │
   ▼
Permissions
```

Permissions are assigned to Roles rather than directly to Users, and Roles are assigned to Users or Groups — making access management scalable.

## Authentication vs Authorization

Authentication verifies user identity and login credentials **before** ServiceNow evaluates the user's roles and applicable ACLs.

---

# Part 3 — Introduction to Access Control Lists (ACLs)

## What is an ACL?

An **Access Control List (ACL)** is a security mechanism that determines **who can access data and what actions they are allowed to perform**. Whenever a user attempts to access a table, record, or field, ServiceNow evaluates the relevant ACLs — access is granted only if all conditions are satisfied.

## Why ACLs Matter

Organizations store sensitive data such as Employee Records, Incident Details, Financial Information, HR Cases, and Customer Data. ACLs ensure users only view or modify what they're authorized to.

## Record-Level vs Field-Level Security

| Type | Controls | Example |
|------|-----------|---------|
| Record-Level ACL | Whether a record can be opened at all | Support Engineer can only view incidents assigned to their group |
| Field-Level ACL | Whether a specific field is visible/editable | User can open an Incident but not view Cost or Work Notes |

## CRUD Operations

| Operation | Description |
|-----------|-------------|
| Create | Create new records |
| Read | View records |
| Write | Modify existing records |
| Delete | Delete records |

Every new custom table automatically receives **four default ACLs** (Create, Read, Write, Delete) plus an associated role.

## ACL Evaluation Components

1. **Operation** — Create, Read, Write, Delete
2. **Object** — the table, field, or wildcard being protected (e.g., `incident`, `incident.short_description`, `incident.*`)
3. **Permissions** — Roles, Conditions, and Scripts that must all be satisfied

## Types of ACLs

```text
incident.None        → Record Access (entire record)
incident.*            → All Fields (wildcard)
incident.priority      → Specific Field
incident.state
incident.caller
```

ServiceNow evaluates ACLs from the most general level down to the most specific.

---

# Part 4 — Creating and Testing ACLs

## Accessing Access Controls

```text
System Security → Access Control (ACL)
```

## Why the "New" Button Was Missing

Even with the **admin** role, the **New** button was not visible. Creating or editing ACLs requires an additional, temporarily elevated role: **security_admin**.

## The security_admin Role

- `admin` → most administrative capabilities.
- `security_admin` → unlocks security-sensitive configurations such as Access Controls.

### Elevating Roles
1. Click the **Profile** icon.
2. Select **Elevate Roles**.
3. Choose **security_admin**.
4. Click **Update**.

## Security Administration Mode — Visual Indicators

- A **red banner/outline** appears at the top of the platform.
- A **red circular outline** surrounds the user profile icon.

These remain visible until the elevated role is removed or expires.

## Practical Scenario — Project Table

| Field |
|--------|
| Project Name |
| Budget |
| Total Expenses |
| Due Date |

| User | Role |
|------|------|
| Siri | Employee |
| Chandransh | Manager |

**Business requirement:** Managers should see all project information; Employees should only see **Project Name** and **Due Date** — sensitive fields like **Budget** and **Total Expenses** must stay hidden from employees.

## Creating the ACL

| Property | Value |
|-----------|-------|
| Type | Record |
| Operation | Read |
| Decision Type | Allow If |

`Allow If` means access is granted only when all configured conditions evaluate successfully; otherwise access is denied.

## Testing via Impersonation

The user **Siri** (Employee role) was impersonated to verify the ACL's behavior without needing her password — one of the safest ways to test security configurations.

**Result:** Since only a Record-Level ACL was configured (not Field-Level), Siri could open the Project record and see **all** fields — demonstrating that Record ACLs and Field ACLs work independently and must both be configured for fine-grained security.

## Field-Level Security (Next Step)

To hide sensitive fields specifically, separate Field ACLs are needed, e.g.:

```text
Project.Budget
Project.Total Expenses
```

## Accessing Tables via Backend Name

Tables can be opened directly without the Application Navigator:

```text
<table_name>.list
```

Examples:
```text
incident.list
u_project.list
u_vip_cohort6.list
```

## Record ACL vs Field ACL — Comparison

| Record ACL | Field ACL |
|------------|-----------|
| Controls access to the entire record | Controls access to a specific field |
| Determines whether a record can be opened | Determines whether a field can be viewed or edited |
| Applied at the row level | Applied at the column level |

---

# Navigation Paths Learned

## Accessing Access Control (ACL)
```text
Application Navigator
    ↓
System Security
    ↓
Access Control (ACL)
```

## Elevating to security_admin
```text
Profile Icon
    ↓
Elevate Roles
    ↓
security_admin
    ↓
Update
```

## Opening a Table via Backend Name
```text
Navigation Bar
    ↓
<table_name>.list
```

## Viewing the Schema Map
```text
Table
    ↓
Related Link: Schema Map
```

---

# Important Terminology

| Term | Description |
|------|-------------|
| Table | A database object where records are stored and managed. |
| Record | A single row inside a table. |
| Field | A single column/attribute inside a table. |
| Core Table | A default, platform-provided table that cannot normally be deleted. |
| Custom Table | An organization-created table that can be deleted. |
| Base Table | The foundational table in a hierarchy. |
| Parent Table | A table from which another inherits. |
| Child Table | A table that inherits fields/behavior from a parent. |
| Schema Map | A graphical tool showing relationships between tables. |
| Extension Relationship | Represents table inheritance in the Schema Map. |
| Reference Relationship | Represents a reference-field link in the Schema Map. |
| RBAC | Role-Based Access Control — permissions assigned via roles. |
| ACL | Access Control List — governs who can perform what action on data. |
| CRUD | Create, Read, Update, Delete operations. |
| security_admin | Elevated role required to create/edit ACLs. |
| Impersonation | Temporarily logging in as another user to test access/behavior. |
| Record-Level ACL | Controls access to an entire record. |
| Field-Level ACL | Controls access to a specific field. |

---

# Practical Activities Performed

- Reviewed the Incident table structure and reinforced Records vs Fields.
- Compared Core Tables and Custom Tables, including deletion rules.
- Attempted to delete the Incident table (no Delete option) vs the VIP Cohort 6 table (Delete option with `delete` confirmation).
- Explored the Task → Incident/Problem/Change Request hierarchy.
- Opened and explored a **Schema Map** for a table, viewing Table Nodes and Relationship Connectors.
- Toggled Extension vs Reference relationship filters on the Schema Map.
- Reviewed the `admin` and `personalize_dictionary` roles required for Schema Map access.
- Navigated to **System Security → Access Control (ACL)**.
- Observed that the **New** ACL button was hidden until elevating to **security_admin**.
- Elevated to **security_admin** via **Profile → Elevate Roles** and observed the red security-mode indicators.
- Created a new **Record-Level ACL** on a custom **Project** table with `Operation: Read` and `Decision Type: Allow If`.
- Impersonated the user **Siri** (Employee role) to test the ACL.
- Observed that without Field-Level ACLs, all fields remained visible even with the Record ACL in place.
- Practiced opening tables directly using backend names with `.list` (e.g., `incident.list`, `u_project.list`).

---

# Key Takeaways

- **Tables** remain the foundation of the ServiceNow platform — every application stores its data as records inside tables.
- ServiceNow ships with **6,000+ Out-of-the-Box tables**, while **Custom Tables** cover organization-specific needs.
- **Core Tables cannot be deleted**; **Custom Tables can be deleted** only after explicit `delete` confirmation.
- Table hierarchy (**Base → Parent → Child**) enables inheritance and reduces duplication, with Task as the classic parent for Incident, Problem, and Change Request.
- The **Schema Map** visually represents **Extension** (inheritance) and **Reference** relationships between tables, and requires the `admin` or `personalize_dictionary` role to access.
- ServiceNow secures data using **Role-Based Access Control (RBAC)**: permissions → roles → groups/users.
- **Access Control Lists (ACLs)** determine who can perform **CRUD** operations on tables, records, and fields.
- Creating or editing ACLs requires elevating to the **security_admin** role — the `admin` role alone is not enough.
- **Record-Level ACLs** and **Field-Level ACLs** are independent and must both be configured for complete, fine-grained security.
- **Impersonation** is a safe, password-free way to test how ACLs behave for a specific user and role.
- Tables can be accessed directly and efficiently using their backend name with the `.list` suffix.

---

# Interview Questions for Revision

### 1. What is the difference between a Core Table and a Custom Table?
### 2. Why can't Core Tables be deleted, while Custom Tables can?
### 3. What is a Schema Map, and what problem does it solve?
### 4. What is the difference between an Extension Relationship and a Reference Relationship?
### 5. What roles are required to view a Schema Map?
### 6. What is Role-Based Access Control (RBAC)?
### 7. What is an Access Control List (ACL), and what does it evaluate?
### 8. Why is the `security_admin` role required to create ACLs, even for an `admin` user?
### 9. What is the difference between a Record-Level ACL and a Field-Level ACL?
### 10. What does the `Allow If` Decision Type mean when creating an ACL?
### 11. How does Impersonation help in testing ACLs?
### 12. How can you open a table directly using its backend name?

---

# Personal Reflection

Today's session tied together two topics that initially felt separate — the **data model** (tables and their relationships) and **security** (ACLs and RBAC). Seeing the Schema Map visually connect tables through inheritance and references made the earlier concept of table extension much more concrete.

The ACL session was especially valuable because it was **hands-on**: creating a Record-Level ACL, elevating to `security_admin`, and then impersonating an Employee to see the ACL's real effect made the abstract idea of "role-based access" feel tangible. It was also an important lesson that a Record ACL alone doesn't hide sensitive fields — Field-Level ACLs are a separate, necessary layer. This distinction is something I'll need to keep in mind while designing secure applications later in the internship.

---

# Conclusion

Day 03 deepened my understanding of how ServiceNow organizes and protects data. Starting from a stronger grasp of **Tables** — including Core vs Custom tables, deletion rules, and inheritance — the session moved into the **Schema Map**, which made table relationships visually intuitive, and finally into **Access Control Lists**, which showed how the platform enforces security through roles, elevation, and layered Record/Field-level permissions.

Together, these three topics form a complete picture: **data is stored in tables, tables are related through inheritance and references, and access to that data is tightly controlled through ACLs and RBAC.** This foundation will be essential as the internship moves toward scripting, automation, and application development in the coming days.

---

# What's Next?

Looking ahead, upcoming sessions are expected to cover:

- Advanced ACL scripting and conditions
- Client Scripts
- Business Rules
- UI Policies
- Data Policies
- Notifications
- Flow Designer
- Service Catalog
- GlideRecord
- Application Development
- Automation & Integrations

---

> **End of Day 03** 🚀
*"Understanding how data is structured and secured is the real foundation of building on ServiceNow — everything else is built on top of tables, relationships, and access control."*
