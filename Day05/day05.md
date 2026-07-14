# Day 05 - CMDB and Knowledge Management Fundamentals

**Date:** 13 July 2026

---

## Introduction

Today's session moved away from the basic administration concepts covered earlier in the internship and introduced two major ServiceNow applications: the **Configuration Management Database (CMDB)** and **Knowledge Management (KM)**.

The session was split into four parts — starting with CMDB fundamentals, moving into how CMDB supports real ITSM processes and the CMDB Workspace, then shifting focus to how organizations create and manage Knowledge Articles, and finally covering more advanced Knowledge Management concepts like article lifecycle actions and user criteria.

---

## Part 1 — CMDB Fundamentals

### What is CMDB?

I learned that the **CMDB (Configuration Management Database)** is a centralized repository in ServiceNow that stores **Configuration Items (CIs)**, their **attributes**, and their **relationships**. It acts as the foundation of IT Asset Management and Relationship Management, giving a complete view of an organization's IT infrastructure.

> **Interview definition:** CMDB is a centralized repository in ServiceNow that stores Configuration Items (CIs), their attributes, and their relationships, enabling impact analysis and supporting IT Service Management (ITSM) processes.

### Why CMDB Matters

CMDB acts as a single source of truth for IT infrastructure. It helps organizations track assets, understand relationships between components, perform impact analysis, improve incident resolution, support change management, and reduce operational risk.

### Configuration Items (CIs)

A **Configuration Item** is any component that needs to be managed to deliver an IT service — servers, computers, routers, switches, firewalls, databases, applications, virtual machines, cloud services, SaaS applications, and business services all count as CIs.

### Attributes vs Relationships

Each CI stores:

- **Attributes** — Name, IP Address, Owner, Status, Location, Serial Number, Manufacturer, Model, OS, Asset Tag.
- **Relationships** — how CIs are connected or depend on each other, e.g.:

```text
Employee Portal
      │
Runs on
      │
Application Server
      │
Connected to
      │
Database Server
```

### Types of CIs

- **Hardware CIs** — servers, computers, laptops, routers, switches, firewalls, storage devices.
- **Software CIs** — business applications, email applications, databases, cloud services, virtual machines, SaaS applications, operating systems.

### Roles and Key Tables

| Role | Purpose |
|------|---------|
| `asset` | Manage IT assets and asset records. |
| `itil` | Perform standard ITSM operations. |
| `itil_admin` | Administer ITSM applications and processes. |
| `cmdb_read` | Read-only access to CMDB records. |

| Table | Purpose |
|--------|---------|
| `cmdb` | Base CMDB framework table. |
| `cmdb_ci` | Stores Configuration Items. |
| `cmdb_rel_ci` | Stores relationships between CIs. |

### Impact Analysis

One of CMDB's biggest advantages is **Impact Analysis** — identifying which CIs and business services will be affected before implementing a change.

**Example:** A network switch is scheduled for maintenance. Using the CMDB, ServiceNow shows it supports the Employee Portal, Finance Application, Email Server, and HR System — so the organization can notify affected users and schedule maintenance during off-hours instead of causing unplanned downtime.

### CMDB vs Asset Management

| Asset Management | CMDB |
|------------------|------|
| Focuses on financial and lifecycle information. | Focuses on technical configuration and relationships. |
| Purchase Date | IP Address |
| Cost | Installed Software |
| Vendor | Dependencies |
| Warranty | Connected Systems |

**Memory tip:** Think of CMDB as the *Google Maps* of an organization's IT infrastructure — CIs are the locations, relationships are the roads, and Impact Analysis predicts the traffic disruptions before you take a route.

---

## Part 2 — CMDB in ITSM & CMDB Workspace

### CMDB Across ITSM Processes

CMDB gives visibility across Incident, Problem, Change, and Request Management by maintaining CIs, attributes, and relationships.

- **Incident Management** — When a server fails and the Employee Portal goes down, CMDB reveals the server also supports Payroll and HR systems, helping prioritize and resolve the incident faster.
- **Problem Management** — Repeated failures on a database server can be traced through relationships to CRM, Customer Portal, and Reporting Dashboard, helping the team fix the root cause instead of repeatedly patching symptoms.
- **Change Management** — Before upgrading a network switch, the Change Manager checks CMDB relationships to Email Server, Finance Application, HR Portal, and Authentication Services, allowing proper impact assessment and scheduling.
- **Request Management** — When an employee requests new software, CMDB validates device compatibility, OS, hardware specs, and existing dependencies before fulfillment.

| ITSM Process | CMDB Helps By |
|--------------|---------------|
| Incident Management | Identifying affected CIs and business services. |
| Problem Management | Finding root cause using CI relationships. |
| Change Management | Performing impact analysis before changes. |
| Request Management | Validating compatibility and dependencies. |

### Dependency View

The **Dependency View** graphically shows how CIs are connected:

```text
Employee Portal
      │
Runs on
      │
Application Server
      │
Connected to
      │
Database Server
      │
Hosted on
      │
Virtual Machine
      │
Runs on
      │
Physical Server
```

If the Physical Server fails, ServiceNow immediately surfaces every affected system above it. Related options include Upstream/Downstream Dependencies, Related CIs, Business Services, and Relationship Details.

### CMDB Workspace

The **CMDB Workspace** is a dedicated interface to search, explore, manage, and analyze CIs from one place instead of navigating multiple modules. It lets administrators search CIs, explore dependency maps, view CI details, analyze relationships, perform impact analysis, update CIs, and monitor CMDB health.

**Real-life example:** A user reports "The Employee Portal is not working." The administrator opens CMDB Workspace, searches for the Employee Portal, opens the Dependency View, finds the connected Application Server, discovers the Database Server is offline, and resolves the root cause quickly.

**Memory tip:** CMDB Workspace is the *control room* of the CMDB — search, explore, analyze, assess impact, and troubleshoot, all from one place.

---

## Part 3 — Knowledge Management Fundamentals

### What is Knowledge Management?

**Knowledge Management (KM)** is a process that lets organizations create, categorize, review, approve, publish, and share knowledge articles through centralized **Knowledge Bases**. The goal is to help employees and customers find accurate information quickly, reduce repetitive support requests, and improve ITSM efficiency.

> **Interview definition:** Knowledge Management in ServiceNow is a centralized system for creating, organizing, approving, publishing, and sharing knowledge articles to improve self-service and support ITSM processes.

### Why KM Matters

It stores organizational knowledge in one place, reduces repetitive incidents, improves self-service, shares best practices, and standardizes documentation — ultimately reducing support costs.

### Access Control

Access can be restricted using **User Criteria** based on roles, departments, groups, locations, or companies. For example, HR articles might be visible only to HR employees, while company policies are visible to everyone.

### Integration with Other Applications

KM integrates with Incident, Problem, Change, and Event Management, as well as Virtual Agent, Service Portal, Employee Center, and Now Mobile. During Incident Management, ServiceNow can automatically suggest relevant Knowledge Articles to help agents resolve issues faster.

### Knowledge Management Roles

- **`knowledge_admin`** — creates Knowledge Bases, configures templates/workflows, manages system-wide settings, controls article lifecycle.
- **`knowledge_manager`** — manages Knowledge Bases, reviews articles, maintains content quality, organizes categories.
- **`knowledge`** — standard users who can create or contribute articles depending on permissions.

### Article Lifecycle

```text
Create
   ↓
Review
   ↓
Approve
   ↓
Publish
   ↓
Update
   ↓
Retire
```

Two publishing approaches:

- **Instant Publish** — no approval needed, published immediately; suited to small teams.
- **Approve → Publish → Retire** — requires approval; better governance, recommended for production.

### Knowledge Base vs Knowledge Article

| Knowledge Base | Knowledge Article |
|----------------|-------------------|
| Repository | Individual document |
| Contains multiple articles | Belongs to one Knowledge Base |
| Organizes knowledge | Provides one solution |
| Managed by Knowledge Managers | Created by Authors |

Each article belongs to only **one** Knowledge Base at a time, and articles use templates — an article can only move to another Knowledge Base if the same template exists there.

### Guided Setup

Knowledge Management can be configured step by step: create Knowledge Bases, assign roles, create workflows, configure properties, configure templates, enable localization, and create ownership groups.

### User Criteria and Localization

User Criteria defines who can access articles based on role, department, group, company, or location. **Localization** allows the same article to be published in multiple languages (e.g., English, Hindi, French, German), and each Knowledge Base can have its own dedicated workflow (e.g., IT KB → IT Approval, HR KB → HR Approval).

**Memory tip:** Think of a library — Knowledge Portal is the building, Knowledge Base is the bookshelf, Knowledge Article is the book, and Category is the book section.

---

## Part 4 — Knowledge Articles, Knowledge Center & Advanced Concepts

### Importing Word Documents

ServiceNow can convert existing Microsoft Word documents into Knowledge Articles directly, saving time on recreating existing documentation:

```
Knowledge → Articles → Import Articles
```

After selecting the target Knowledge Base and Category, the file is uploaded (browse or drag-and-drop) and imported for review before publishing.

### Knowledge Portal vs Knowledge Center

- **Knowledge Portal** — where users browse, search, and access Knowledge Bases and Articles; shows counts of KBs/articles, categories, and recently updated articles.
- **Knowledge Center** — the workspace where users create, edit, publish, and maintain articles. Selecting a Knowledge Base first generates a unique **Knowledge Number** (e.g., `KB0010001`).

### Key Fields on an Article

| Field | Purpose |
|--------|---------|
| Knowledge Number | Auto-generated unique article ID. |
| Short Description | Title of the article. |
| Article Body | Main content. |
| Knowledge Base | Repository where it's stored. |
| Category | Organizes the article within the KB. |
| Publish Date | When the article becomes available. |
| Valid To | Validity period. |
| Owner | Person responsible. |
| Workflow | Current state (Draft, Published, Retired, etc.). |

Fields like **Source Task** and **Version** are read-only and managed automatically by the platform.

### Lifecycle Actions I Learned

- **Flag** — notifies Knowledge Managers that an article may be incorrect, outdated, or incomplete. It does *not* delete or retire the article.
- **Recall** — removes a published article from circulation and sends it back into the workflow, keeping the same Knowledge Number.
- **Checkout / Check In** — Checkout creates an editable working copy of a published article (preventing simultaneous edits); Check In saves the changes and pushes it back through the configured workflow.
- **Retire vs Replace** — Retire makes an article inactive with no replacement; Replace retires the article *and* redirects users to a newer one.
- **Republish** — a retired article can be republished, either instantly or through an approval workflow, keeping the same Knowledge Number.

### Two Separate User Criteria Modules

I found it interesting that ServiceNow has two distinct User Criteria modules:

- **Knowledge → Administration → User Criteria** — controls access to Knowledge Bases and Articles.
- **Service Catalog → User Criteria** — controls access to Catalog Items, Record Producers, and Catalog Categories.

### Approval Tasks and Impersonation

Approval tasks are generated **before** publication, not after:

```text
Create Article
      ↓
Approval Task
      ↓
Reviewer Approval
      ↓
Published
```

When impersonating a user, ServiceNow evaluates that user's roles and User Criteria — if they don't meet the criteria for a Knowledge Base, that KB and its articles simply won't appear for them.

### Knowledge on Now Mobile

Knowledge Management is also available on the **Now Mobile** app, letting users browse KBs, search articles, view published content, rate articles, and give feedback, provided the right mobile plugin is installed.

**Memory tip:** Think of the whole KM lifecycle like publishing a book — Draft is writing it, Review/Approve is the editor's pass, Publish puts it on the shelf, Recall pulls it back for corrections, Checkout/Check In is editing a new edition, Replace swaps in a new edition, Retire takes it out of circulation, and Republish puts it back.

---

## Key Takeaways

- CMDB structure: Configuration Items, Attributes, Relationships
- `cmdb`, `cmdb_ci`, `cmdb_rel_ci` tables
- CMDB roles: `asset`, `itil`, `itil_admin`, `cmdb_read`
- Impact Analysis and Dependency View
- CMDB Workspace as a centralized management interface
- CMDB's role across Incident, Problem, Change, and Request Management
- CMDB vs Asset Management distinction
- Knowledge Management fundamentals and roles (`knowledge_admin`, `knowledge_manager`, `knowledge`)
- Knowledge Base vs Knowledge Article
- Article lifecycle: Create → Review → Approve → Publish → Update → Retire
- Instant Publish vs Approval Workflow
- Importing Word documents as Knowledge Articles
- Knowledge Portal vs Knowledge Center
- Flag, Recall, Checkout, Check In, Retire, Replace, Republish
- Two separate User Criteria modules (Knowledge vs Service Catalog)
- Impersonation and its effect on visible Knowledge Bases
- Knowledge Management on Now Mobile

---

## Reflection

Today's session felt like a big step up in complexity compared to the earlier sessions. CMDB introduced me to how ServiceNow actually maps out an entire IT environment — not just as a list of assets, but as a living network of dependencies that can be used to predict the impact of a single failure or planned change before it happens.

Knowledge Management, on the other hand, showed me how ServiceNow handles something that seems simple on the surface — documentation — but actually has a surprisingly rich lifecycle behind it, with governance, access control, and workflows built in to keep information accurate and relevant.

Together, these two topics reinforced a pattern I'm starting to notice throughout the internship: ServiceNow's real strength isn't any single feature, but how tightly everything is connected — CIs feed into Incident/Problem/Change processes, and Knowledge Articles feed back into those same processes to help resolve issues faster. I'm looking forward to seeing how these connect further as the internship progresses.
