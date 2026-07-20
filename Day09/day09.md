# Day 09 - Business Rules in ServiceNow

**Date:** 18 July 2026

---

## Introduction

Day 08 covered **UI Actions**—automation triggered manually by a user clicking a button, link, or menu option. Day 09 shifted back to system-driven automation with **Business Rules**, the most powerful server-side automation feature in ServiceNow. Where a UI Action waits for a click, a Business Rule fires automatically whenever a record is created, updated, deleted, queried, or displayed—no user action required.

The session began with the theory behind Business Rules—what they are, why they matter, and the anatomy of a rule (Name, Table, When, Order, Condition, Action, Advanced, Script). This was followed by a deep dive into each execution type—**Before**, **After**, **Async**, and **Display**—with real-world scenarios and sample scripts for each. The day closed with a full comparison of all four types, best practices, common mistakes, and an interview-focused revision sheet.

---

## Why Business Rules Matter

Imagine an organization where employees manually perform the same repetitive actions every time an Incident is created—assigning category, priority, state, assignment group, and sending a notification. If every employee does this by hand:

- Human errors increase.
- Records become inconsistent.
- Productivity decreases.
- Reporting becomes inaccurate.
- Service delivery slows down.

Business Rules eliminate this manual repetition by automatically enforcing organizational policy. Instead of depending on users to remember the rules, ServiceNow depends on automation.

**Without Business Rules** — an employee reports Outlook not working, and a support agent must manually open the record, change the category, assign the software team, change the state, and send a notification. Every engineer repeats these same steps, thousands of times a day.

**With Business Rules** — the same sequence happens automatically within milliseconds the moment the Incident is created:

```text
Employee Creates Incident
        ↓
Business Rule Executes
        ↓
Category = Software
        ↓
Assignment Group = Software
        ↓
State = In Progress
        ↓
Notification Sent
```

---

## What is a Business Rule?

A **Business Rule** is a server-side script that automatically executes when a specific database operation occurs on a record: Create (Insert), Update, Delete, Query, or Display.

> **Interview Definition:**
> A **Business Rule** is a server-side automation that executes whenever a record is created, updated, deleted, queried, or displayed, to enforce business logic and maintain data integrity.

Because Business Rules run on the server, users cannot see, modify, disable, or bypass them from the browser—unlike client-side logic.

### Client Side vs Server Side

| Client Side | Server Side |
|-------------|-------------|
| Runs inside the user's browser | Runs on the ServiceNow server |
| Visible to users | Hidden from users |
| Faster for UI interactions | Better for database operations |
| Can be bypassed | Cannot be bypassed |
| Examples: Client Scripts, UI Policies | Examples: Business Rules, Script Includes |

---

## Core Concepts

**ServiceNow Customization** — every organization has unique processes (a hospital manages patients differently than a bank), and Business Rules let ServiceNow's default behavior be customized without altering the platform itself—auto-assigning regional teams, blocking incomplete closures, updating parent records, calculating values dynamically.

**Record Operations** — Business Rules trigger on five operations:

| Operation | Trigger Example |
|-----------|------------------|
| Create (Insert) | A new Incident is created |
| Update | Priority is changed |
| Delete | A Knowledge Article is deleted |
| Query | A user opens the Incident list |
| Display | The Incident form opens |

**The `sys_script` Table** — every Business Rule created in ServiceNow is stored in the **`sys_script`** table, holding its Name, Table, Active status, When, Order, Filter Conditions, Script, and Advanced settings.

### Business Rule Workflow

```text
User Creates or Updates Record
            ↓
      Business Rule Checks
            ↓
      Condition Evaluated
            ↓
      ┌─────┴─────┐
      ▼           ▼
    TRUE        FALSE
      ↓           ↓
   Execute     Skip Rule
      ↓
Database Updated
```

---

## Business Rule Anatomy

A Business Rule is built from the following components:

| Field | Purpose |
|--------|---------|
| Name | Identifies the rule, e.g. `Auto Assign Software Team` (avoid vague names like `Rule1` or `Test`) |
| Table | Where the rule runs, e.g. `Incident`, `Problem`, `Change Request`, `Knowledge`, `Task` |
| When | Execution type: Before, After, Async, or Display |
| Order | Sequence when multiple rules run on the same table/operation—lower values run first |
| Condition | Determines whether the script runs, e.g. `current.priority == 1` |
| Action | Simple, scriptless actions such as setting field values or making fields mandatory |
| Advanced | Enables the scripting section for full JavaScript logic |
| Script | The JavaScript executed, with access to `current`, `previous`, `gs`, and `GlideRecord` |

**Advanced = False** — no scripting required; uses conditions and actions only; suitable for simple automation.
**Advanced = True** — JavaScript enabled; full access to Business Rule APIs; supports complex logic across records and tables. Most real-world Business Rules use Advanced = True.

**Sample Script**

```javascript
(function executeRule(current, previous) {
    if (current.priority == 1) {
        current.assignment_group = "Software";
    }
})(current, previous);
```

---

## Before Business Rules

A **Before Business Rule** executes **before** the record is committed to the database. Since the record hasn't been written yet, any changes made to `current` are automatically included in the same save—no extra database update needed. This makes it the most common and efficient type, ideal for validation, default values, and field modification.

```text
User Creates or Updates Record
            ↓
Before Business Rule Executes
            ↓
Validate / Modify Record
            ↓
Database Save Occurs
```

**Example 1 – Network Category Validation.** Every Incident related to network issues must belong to the **Network** category. If the Short Description contains the word "network," the Business Rule corrects the category before saving:

```javascript
(function executeRule(current, previous) {
    if (current.short_description.toLowerCase().includes("network")) {
        current.category = "network";
    }
})(current, previous);
```

**Example 2 – State Automation.** Whenever a support engineer adds Work Notes, the Incident should automatically move to **In Progress**, so engineers never forget to update status:

```javascript
(function executeRule(current, previous) {
    if (current.work_notes.changes()) {
        current.state = 2;
    }
})(current, previous);
```

**Limitations** — Before Business Rules should not be used for notifications, long-running tasks, integrations, or heavy processing; those belong in After or Async rules.

---

## After Business Rules

An **After Business Rule** executes **after** the record has already been committed to the database. Because the record already exists, these rules are used for actions that depend on a successful save: notifications, related record updates, child record creation, triggering events, and audit logging.

```text
User Saves Record
        ↓
Record Saved
        ↓
After Business Rule Executes
        ↓
Additional Processing
```

**Example 1 – Creation Confirmation.** Log an informational message once an Incident has been successfully saved, since its number is now available:

```javascript
(function executeRule(current, previous) {
    gs.info("Incident " + current.number + " has been created successfully.");
})(current, previous);
```

**Example 2 – On-Hold Processing.** When an Incident's state changes to **On Hold**, update all related child tasks:

```javascript
(function executeRule(current, previous) {
    if (current.state.changesTo(3)) {
        var task = new GlideRecord("task");
        task.addQuery("parent", current.sys_id);
        task.query();
        while (task.next()) {
            task.state = 3;
            task.update();
        }
    }
})(current, previous);
```

**Limitations** — avoid using After Business Rules for simple validation or default values, and avoid repeatedly updating the same record inside one, which can cause recursive execution.

### Before vs After

| Feature | Before Business Rule | After Business Rule |
|---------|----------------------|---------------------|
| Runs | Before database save | After database save |
| Record exists in database | No | Yes |
| Best for | Validation and field updates | Notifications and related processing |
| Database update required | No | Usually yes, for related records |
| Performance | Faster | Slightly slower |

---

## Async Business Rules

An **Async (Asynchronous) Business Rule** also executes after the database transaction completes, but unlike an After Business Rule, it does not make the user wait. ServiceNow places it in a background queue processed independently by the scheduler, so the form returns immediately while processing continues.

```text
User Saves Record
        ↓
Record Saved → User Continues Working
        ↓
Async Business Rule Added to Background Queue
        ↓
Scheduler Executes Rule
```

This is ideal when saving a record could otherwise trigger several slow operations—multiple emails, a REST API call, hundreds of related record updates, or report generation—that shouldn't block the user.

**Example – High Priority Notifications.** When Incident Priority becomes Critical, several stakeholders need to be notified. Because delivery may take time, the notification is queued as an event rather than sent inline:

```javascript
(function executeRule(current, previous) {
    if (current.priority == 1) {
        gs.eventQueue(
            "incident.high.priority",
            current,
            current.number,
            gs.getUserID()
        );
    }
})(current, previous);
```

**Limitations** — do not use Async rules when the result is needed immediately, when a later rule depends on the output, or when data must be validated or modified before saving. Those belong in a Before Business Rule.

---

## Display Business Rules

A **Display Business Rule** executes **before a form is displayed**. Unlike the other three types, it never updates database records—its job is to prepare information the client side needs while the form loads, often bridging server-only data to Client Scripts via `g_scratchpad`.

```text
User Opens Form
        ↓
Display Business Rule Executes
        ↓
Server Retrieves Data
        ↓
Information Passed to Client
        ↓
Form Displayed
```

**Example – Form Messaging.** Show a welcome/reminder message the moment an Incident opens:

```javascript
(function executeRule(current, previous) {
    gs.addInfoMessage(
        "Review the incident details before making any updates."
    );
})(current, previous);
```

**Example – Dynamic Behavior Using `g_scratchpad`.** A Client Script needs to know whether an Incident belongs to a VIP caller. The server retrieves this and stores it in `g_scratchpad` so the Client Script can use it without a separate Ajax call:

```javascript
(function executeRule(current, previous) {
    g_scratchpad.isVIP = current.caller_id.vip;
})(current, previous);
```

**Limitations** — Display Business Rules should never update records, perform validation, send notifications, or run heavy calculations; their responsibility is limited to preparing data for the UI.

### Async vs Display

| Feature | Async Business Rule | Display Business Rule |
|---------|----------------------|-----------------------|
| Executes | After database save | Before form display |
| User waits? | No | Yes (before form loads) |
| Purpose | Background processing | Prepare data for UI |
| Updates records | Yes | No |
| Uses scheduler | Yes | No |
| Common use | Notifications, integrations | Messages, `g_scratchpad` |

---

## Execution Sequence

Understanding *when* each type executes is one of the most common interview topics. On a record modification:

```text
User Performs Action
        ↓
Before Business Rule
        ↓
Database Operation
        ↓
After Business Rule
        ↓
Async Business Rule (Background Scheduler)
```

On simply opening a record without modifying it:

```text
User Opens Form
        ↓
Display Business Rule
        ↓
Form Displayed
```

### Complete Comparison

| Feature | Before | After | Async | Display |
|---------|---------|--------|--------|---------|
| Executes | Before database save | After database save | After database save (background) | Before form display |
| User waits? | Yes | Yes | No | Yes |
| Database updated? | Yes | Already updated | Already updated | No |
| Validation | Best choice | No | No | No |
| Default values | Yes | No | No | No |
| Notifications | No | Yes | Best choice | No |
| Related record updates | Limited | Yes | Yes | No |
| Heavy processing | No | Limited | Yes | No |
| Uses scheduler | No | No | Yes | No |
| Uses `g_scratchpad` | No | No | No | Yes |
| Main purpose | Validate & modify data | Trigger post-save actions | Background processing | Prepare UI data |

### Choosing the Correct Type

```text
Need to validate data?         → Use BEFORE
Need record to already exist?  → Use AFTER
Long-running task?             → Use ASYNC
Need data before form loads?   → Use DISPLAY
```

---

## Best Practices Learned

- Choose the execution type based on the requirement—validation and defaults belong in **Before**; notifications and related updates belong in **After** or **Async**; UI prep belongs in **Display**.
- Keep each Business Rule focused on a single responsibility rather than combining assignment, email, task updates, and API calls into one script.
- Always use conditions (e.g. `current.priority == 1`) so rules skip unnecessary execution instead of running on every record.
- Avoid recursive updates—calling `current.update()` inside a Business Rule can re-trigger the same rule; prefer modifying `current` directly inside a Before Business Rule instead.
- Use descriptive names (`Auto Assign Software Group`, not `Rule1` or `Test`) to keep the system maintainable.
- Keep scripts small; move reusable or complex logic into Script Includes, Flow Designer, or shared utilities.
- Always test different priorities, states, insert/update operations, and edge cases—including how multiple Business Rules interact—before deploying to Production.

---

## Common Mistakes

- Using an **After Business Rule** to validate data — validation belongs in a **Before Business Rule**.
- Using an **Async Business Rule** for immediate validation — validation must happen before the record saves.
- Using a **Display Business Rule** to update records — Display rules should never modify the database.
- Ignoring the **Order** field when multiple Business Rules exist for the same table and operation.
- Creating duplicate Business Rules for the same purpose instead of reusing and consolidating existing logic.

---

## Interview Questions & FAQs

**What is a Business Rule?**
A server-side script that automatically executes when records are created, updated, deleted, queried, or displayed, to enforce business logic and maintain data integrity.

**Where are Business Rules stored?**
In the **`sys_script`** table.

**Which Business Rule executes first?**
The **Before Business Rule**, since it runs prior to the database operation.

**Which Business Rule is best for validation?**
The **Before Business Rule**.

**Which Business Rule should be used for notifications?**
Generally **Async Business Rules**, since they run in the background; simpler **After Business Rules** can also work for lighter post-save actions.

**Which Business Rule executes in the background?**
The **Async Business Rule**.

**Which Business Rule is used with `g_scratchpad`?**
The **Display Business Rule**.

**Why is a Before Business Rule more efficient than an After Business Rule for updating field values?**
Because the record hasn't been committed yet—changes to `current` become part of the same database transaction, avoiding a second update.

**Can Display Business Rules update database records?**
No. Their purpose is to prepare data for the user interface only.

---

## Memory Map

```text
Business Rules
        │
        ├── Before
        │       ├── Validation
        │       ├── Default Values
        │       ├── Auto Assignment
        │       └── Data Modification
        │
        ├── After
        │       ├── Notifications
        │       ├── Child Records
        │       ├── Related Updates
        │       └── Events
        │
        ├── Async
        │       ├── Background Processing
        │       ├── Integrations
        │       ├── Emails
        │       ├── Logging
        │       └── Heavy Tasks
        │
        └── Display
                ├── Messages
                ├── g_scratchpad
                ├── UI Preparation
                └── Client Data
```

### Quick Revision Sheet

```text
Stored In                    sys_script
Runs On                      Server
Best for Validation          Before
Best for Notifications       Async
Best for Related Records     After
Best for UI Preparation      Display
Uses g_scratchpad            Display
Background Execution         Async
Record Exists?               After & Async
Executes Before Save         Before
Executes Before Form Opens   Display
```

---

## Key Takeaways

- Business Rules automate server-side logic during insert, update, delete, query, and display operations.
- **Before Business Rules** validate and modify data before it's saved—efficient, since no extra update is needed.
- **After Business Rules** run once a record has been committed, ideal for notifications and related-record processing.
- **Async Business Rules** move long-running tasks to the background scheduler, improving the user's save experience.
- **Display Business Rules** prepare server-side data for the UI and Client Scripts via `g_scratchpad`, and never touch the database.
- Choosing the correct execution type—and following practices like using conditions and avoiding recursive updates—keeps automation performant and maintainable.

---

## Reflection

Day 09 felt like the natural counterpart to Day 08. Where UI Actions put control in the user's hands through a button they choose to click, Business Rules run silently and automatically the moment a record is touched—no one can forget to trigger them, and no one can bypass them from the browser. That reliability is exactly why they're considered one of the most important server-side tools in ServiceNow.

The most useful mental model from today was mapping each execution type to a single question: does this need to happen *before* the save (validation), *after* the save (something that depends on the record existing), *in the background* (something slow), or *before the form even opens* (something the UI needs)? Once that question has an answer, the correct Business Rule type follows almost automatically.

```text
Day 08 → UI Actions (user-triggered)
             ↓
Day 09 → Business Rules (automatic, database-triggered)
```

Together, these two days show both halves of ServiceNow's automation toolkit: buttons that wait for a person to click them, and rules that never wait for anyone at all. This repository will serve as my daily learning journal throughout the AICTE × ServiceNow Virtual Internship Program, documenting both theoretical concepts and practical exercises as I continue exploring the ServiceNow ecosystem.
