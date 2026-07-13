# Day 04 - Import Sets, Transform Maps, Coalesce Fields, and Plugins

**Date:** 11 July 2026

---

## Introduction

Today's session moved from platform basics into how ServiceNow actually gets data **into** the system and how the platform's own functionality can be **extended**. We covered four major concepts: Import Sets, Transform Maps, Coalesce Fields, and Plugins (including the Personal Developer Instance, or PDI).

Where Day 01 focused on how ServiceNow organizes and displays data (tables, forms, users, roles), today was about how external data enters ServiceNow safely, and how the platform itself can be expanded with new functionality.

---

## Import Sets and Data Loading

### What is an Import Set?

An **Import Set** is how ServiceNow brings data in from external sources — instead of writing straight into a production table, the data lands first in a temporary **Import Set Table (staging table)**, gets validated, and only then moves into the real target table.

### Why stage the data first?

Staging gives administrators a chance to:

- Validate incoming data
- Clean incorrect records
- Map fields correctly
- Prevent duplicate records
- Control exactly how data enters ServiceNow

### Import Set Architecture

```
External Data Source
        |
        ▼
   Data Source
        |
        ▼
Import Set Table
 (Temporary Storage)
        |
        ▼
  Transform Map
        |
        ▼
   Target Table
 (ServiceNow Database)
```

### Step 1 — Data Source

The very first step of any import is creating (or selecting) a **Data Source**. It defines where the data is coming from, what format it's in, and how ServiceNow should read it.

Supported types I learned about:

| Category | Examples |
|---|---|
| File-based | CSV, Excel (`.xls`, `.xlsx`), XML |
| Database | JDBC (Oracle, MySQL, SQL Server) |
| Directory | LDAP (e.g., Active Directory → `sys_user`) |
| API-based | REST, SOAP |

### Full Import Process Flow

```
Create Data Source
        |
        ▼
Load Data into Import Set Table
        |
        ▼
Create Transform Map
        |
        ▼
Transform Data
        |
        ▼
Store Data in Target Table
```

### Worked example

**Employee_Data.csv**

| Employee ID | Name | Email |
|---|---|---|
| 101 | John | john@test.com |

This lands first in an Import Set Table (e.g. `u_employee_import`) — the data is **not yet** in a real ServiceNow table at this point. Only after the Transform Map runs does it reach a target table like `sys_user`, `incident`, or `cmdb_ci`.

### Common Import Set Use Cases

| Use Case | Source | Target Table |
|---|---|---|
| User Import | HR Database | `sys_user` |
| Incident Import | External Helpdesk | `incident` |
| CMDB Import | Discovery Tool | `cmdb_ci` |

---

## Transform Maps

### What is a Transform Map?

A **Transform Map** defines how data moves from the Import Set Table into the Target Table — it handles field mapping, transformation logic, and whether records get created or updated.

```
Import Set Table
        |
        ▼
  Transform Map
        |
        ▼
   Target Table
 (ServiceNow Database)
```

### Why it's needed

Incoming data rarely matches ServiceNow's internal field names. For example:

**External HR System**

| Field | Value |
|---|---|
| Employee_Name | John Smith |
| Employee_Email | john@test.com |

**ServiceNow `sys_user` table**

| Field | Value |
|---|---|
| Name | John Smith |
| Email | john@test.com |

The Transform Map is what tells ServiceNow:

```
Employee_Name  →  Name
Employee_Email →  Email
```

### Components of a Transform Map

1. **Source Table** — the Import Set Table (e.g. `u_employee_import`)
2. **Target Table** — the final table (`sys_user`, `incident`, `cmdb_ci`, etc.)
3. **Field Maps** — the source-to-target field relationships
4. **Transform Scripts** — custom logic that runs during transformation

### Transform Script types

| Script | Runs | Typically used for |
|---|---|---|
| `onStart` | Before transformation begins | Initial setup, preparing variables |
| `onBefore` | Before a record is inserted/updated | Validation, modifying incoming values |
| `onAfter` | After a record is inserted/updated | Additional processing, triggering actions |
| `onComplete` | After the whole transform finishes | Cleanup, final processing |

---

## Coalesce Fields

### What is a Coalesce Field?

A **Coalesce Field** is how ServiceNow decides whether an incoming record already exists — it acts as a unique identifier during transformation, deciding between an **insert** and an **update**.

### How it works

```
   Imported Record
         |
         ▼
 Check Coalesce Field
         |
         ▼
Does Matching Record Exist?
     /          \
   Yes           No
    |             |
  UPDATE       INSERT
 Existing       New
  Record        Record
```

### Match found → update

| Name | Email |
|---|---|
| John | john@test.com |

Incoming record with the same email (`john@test.com`) → **existing record is updated**.

### No match → insert

Incoming record with `alex@test.com`, which doesn't exist yet → **new record is inserted**.

### Multiple Coalesce fields

More than one field can be used together as a composite key — e.g. **Employee ID + Email** must both match before ServiceNow treats it as the same record.

### Why it matters

| Without Coalesce | With Coalesce |
|---|---|
| Every import creates a new record | Existing records are updated, new ones inserted |
| Leads to duplicate users/CIs | Prevents duplicates |
| Data gets messy over time | Keeps the database clean and accurate |

### Transform Map vs Coalesce Field

| Transform Map | Coalesce Field |
|---|---|
| Defines how data moves | Identifies records |
| Maps fields | Prevents duplicates |
| Controls transformation logic | Decides insert vs. update |
| Required for transformation | Optional, but strongly recommended |

---

## Plugins

### What is a Plugin?

A **Plugin** is a software component that extends ServiceNow's functionality — it can add applications, modules, tables, roles, workflows, UI components, and more, without building everything from scratch.

### Why organizations use them

- Extend existing functionality
- Enable additional ServiceNow products
- Add business applications (e.g. HR Service Delivery)
- Improve automation and integrations
- Add demo data for learning/testing
- Reduce custom development effort

### Example

If an organization wants to manage employee requests but the base instance lacks HR features, an admin can activate the **HR Service Delivery Plugin**, which brings in HR applications, workflows, modules, and tables.

### Plugin Activation Process

```
Open Plugins Page
        |
        ▼
  Search Plugin
        |
        ▼
Check Dependencies
        |
        ▼
 Activate Plugin
        |
        ▼
Configure Features
```

Plugins are managed from:

```
All → System Definition → Plugins
```

### Plugin status values

| Status | Meaning |
|---|---|
| Active | Installed and available |
| Inactive | Exists but not currently enabled |
| Pending Installation | Activation has been requested |

### What a plugin can add

| Component | Example |
|---|---|
| Applications | Customer Service Management |
| Modules | HR Cases, Knowledge |
| Tables | HR Case Table, Customer Account Table |
| Workspaces | Agent Workspace, HR Agent Workspace |
| Demo Data | Sample users, incidents, requests |

### Plugin vs Application

| Plugin | Application |
|---|---|
| Extends the ServiceNow platform | Provides business functionality |
| An installation component | A user-facing solution |
| Can install applications | Built from tables and modules |
| Managed via Plugins page | Managed via Application Navigator |

---

## Personal Developer Instance (PDI)

A **PDI** is a free ServiceNow environment for learning and development, letting anyone practice administration and configuration without needing a company instance.

Useful for:

- Learning ServiceNow administration
- Practicing CSA exam concepts
- Building and testing custom applications
- Exploring platform features and scripting

Most learning-related plugins can be activated in a PDI without extra licensing, though some advanced/enterprise plugins still require paid subscriptions or specific products.

---

## Complete Flow: Import → Transform → Plugin

```
External Data
      |
      ▼
  Data Source
      |
      ▼
Import Set Table
      |
      ▼
 Transform Map
      |
      ▼
Coalesce Check
      |
      ▼
 Target Table


ServiceNow Instance
      |
      ▼
Plugin Activation
      |
      ▼
New Applications
New Modules
New Features
New Capabilities
```

---

## Key Takeaways

- Import Sets bring external data into ServiceNow through a safe, staged process.
- A Data Source is always the first step of any import.
- Import Set Tables are temporary — data isn't "real" until it's transformed.
- Transform Maps map staging fields to target table fields, with optional Transform Scripts (`onStart`, `onBefore`, `onAfter`, `onComplete`) for custom logic.
- Coalesce Fields prevent duplicate records by deciding between insert and update.
- Plugins extend the platform itself — applications, modules, tables, workspaces, and demo data — without custom-building everything.
- A PDI is the sandbox where most of this can be safely practiced for free.

---

## Reflection

If Day 01 was about how ServiceNow stores and displays information, today was about how information *gets there* in the first place, and how the platform itself can grow beyond its default capabilities. Import Sets, Transform Maps, and Coalesce Fields together form a clear pipeline — source, staging, transformation, and target — while Plugins showed me that ServiceNow isn't a fixed product but a platform that keeps expanding based on what an organization actually needs.

These two concepts — data loading and platform extensibility — feel like a natural next layer on top of the foundational admin concepts from Day 01, and I'm looking forward to seeing how they connect with scripting and automation in the sessions ahead.
