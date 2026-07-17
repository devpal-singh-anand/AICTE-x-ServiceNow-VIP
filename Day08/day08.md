# Day 08 - UI Action Fundamentals & Client-Server Automation

**Date:** 17 July 2026

---

## Introduction

Day 07 focused on **Flow Designer**—automation that runs in the background whenever a record is created or updated, without any direct user action. Day 08 flipped that perspective around and explored **UI Actions**, which are triggered manually by a user clicking a button, link, or menu option. Together, these two days cover both sides of ServiceNow automation: system-driven automation through Flows, and user-driven automation through UI Actions.

The session began with the theory behind UI Actions—what they are, why they're used, and the many places they can appear on a form or list. This was followed by a complete hands-on exercise creating a custom **VIP** UI Action on the Incident table, enabling every display option, adding a test script, and verifying it in the browser. The day closed with a deeper look at how UI Actions execute—specifically the difference between **client-side** and **server-side** logic—along with how a single UI Action can combine both, and how UI Actions compare to Client Scripts and Business Rules.

---

## What is a UI Action?

A **UI Action** in ServiceNow is a configuration element used to create interactive actions on forms and lists. UI Actions allow users to perform operations by clicking buttons, links, context menu options, or list actions—instead of navigating through multiple menus to achieve the same result.

> **Interview Definition:**
> A **UI Action** is a ServiceNow configuration used to create buttons, links, and context menu options that allow users to perform actions directly on records.

**Simple Example**

On an Incident form, alongside the standard Save, Update, and Close Incident buttons, a custom **VIP** button can be added. Clicking it triggers the following flow:

```text
VIP Button
      ↓
UI Action executes
      ↓
Script runs
      ↓
Action is performed
```

### Why Do We Use UI Actions?

UI Actions make ServiceNow applications more user-friendly and efficient in three key ways:

**1. Provide Quick Actions** — Users can perform tasks directly from a record instead of navigating through several screens. For example, instead of opening an Incident, manually changing its state, and updating the record, a single "Close Incident" button collapses the whole sequence into one click.

**2. Automate Record Operations** — A UI Action can update fields, change record states, create related records, trigger workflows, or redirect users. For example, an "Approve Request" button can change the approval state and notify the requester in one click.

**3. Improve User Experience** — Users don't need any technical knowledge; a simple button hides complex backend operations. For example, an "Assign to Support Team" button might silently update the Assignment Group, update the Assigned User, and send a notification, all behind the scenes.

---

## Where Can UI Actions Appear?

A developer decides where a UI Action should appear at the time it's created. UI Actions can be displayed in several locations across a form or list:

| Location | Description | Example |
|-----------|-------------|---------|
| Form Button | A button displayed directly on a record form | Save \| Update \| Close Incident \| VIP |
| Form Link | A clickable link displayed on the form | Assign Incident, Create Problem, VIP |
| Related Link | A link displayed under the Related Links section | Create Problem, View SLA, VIP |
| Context Menu | An action available through the right-click menu on a record | Open, Update, Close, VIP |
| List Banner Button | A button displayed at the top of a list view; can work on selected records | New \| Delete \| VIP |
| List Bottom Button | A button displayed at the bottom of a list | VIP (below INC001, INC002, INC003) |
| List Context Menu | A right-click action available on records in list view | Open, Assign, Delete, VIP |
| List Choice | An option available in list choice menus when working with list records | VIP |
| List Link | A clickable link displayed in list views | VIP |

---

## UI Action Components

A UI Action form contains several important configuration fields:

| Field | Purpose |
|--------|---------|
| Name | The display name of the UI Action, e.g. `VIP`—this is what users see |
| Table | Defines where the UI Action will be available, e.g. `Incident [incident]` restricts it to Incident records only |
| Action Name | The internal name ServiceNow uses to identify the action, e.g. `VIP` |
| Active | Controls whether the UI Action is enabled (checked) or disabled (unchecked) |
| Show Insert | Controls whether the UI Action appears while creating a new record |
| Show Update | Controls whether the UI Action appears when updating an existing record |
| Script | Defines what happens when the UI Action is executed, e.g. `alert("VIP Button Clicked!");` |

### UI Action Execution Flow

```text
User opens record
        ↓
UI Action is displayed
        ↓
User clicks button/link
        ↓
UI Action script executes
        ↓
Client or Server logic runs
        ↓
Action completes
```

### UI Action vs Normal Button

A normal button only displays something on screen. A ServiceNow **UI Action** is a button combined with logic, a database operation, and automation—it's an interactive trigger, not just a visual element.

---

## Hands-on: Creating the VIP UI Action

**Scenario:** Create a custom UI Action named **VIP** on the Incident table. When clicked, it should appear in multiple locations on the form and list, and display a message confirming it works.

```text
User clicks VIP
        ↓
UI Action executes
        ↓
Alert message appears
```

**Navigation**

```text
All
└── System UI
    └── UI Actions
        └── New
```

**Basic Configuration**

- **Name** → `VIP` — the label users see, e.g. alongside Save | Update on the Incident form.
- **Table** → `Incident [incident]` — restricts the action to Incident records only; it will not appear on Problem, Change, or User tables.
- **Action Name** → `VIP` — the internal identifier ServiceNow uses to reference the action.

**Display Options Enabled**

All of the following checkboxes were enabled to make VIP appear everywhere: Context Menu, Form Link, Form Button, Related Link, List Banner Button, List Bottom Button, List Context Menu, List Choice, and List Link. In addition:

- **Active** — enabled, so the UI Action is visible to users. If left unchecked, the UI Action would exist in the system but remain invisible.
- **Show Insert** — enabled, so VIP appears when creating a new Incident.
- **Show Update** — enabled, so VIP appears when opening an existing Incident.

**Script**

For this initial demo, a simple client-side script was used to confirm the button works:

```text
alert("VIP Button Clicked!");
```

Clicking **Submit** creates the UI Action.

**Testing**

Opening an existing Incident (e.g. `INC0010001`) or creating a new one confirmed VIP appeared as a Form Button, under Related Links, in the right-click Context Menu, and in the List View depending on the enabled options. Clicking it displayed the alert `VIP Button Clicked!`, confirming the click event correctly triggers the script.

### Real-World Version of the Same Action

Instead of just showing an alert, a production-ready VIP action would actually update the record:

```text
Click VIP
      ↓
Set VIP field = True
      ↓
Save record
      ↓
Show confirmation message
```

```text
current.u_vip = true;
current.update();
gs.addInfoMessage("Incident marked as VIP");
```

### Common Mistakes During UI Action Creation

- **Wrong table selected** — e.g. choosing `Task` instead of `Incident`, causing VIP to not appear where expected.
- **Active checkbox forgotten** — the UI Action remains inactive and invisible to users.
- **Display location not enabled** — e.g. only Form Button checked, so the action never appears in Related Links or the Context Menu.
- **Script syntax errors** — e.g. `alert(VIP Button Clicked);` without quotes, instead of the correct `alert("VIP Button Clicked!");`.

---

## Client-side vs Server-side UI Actions

A UI Action can execute its logic in one of two places, depending on what the operation actually needs to do.

### Client-side UI Action

A **client-side** UI Action runs in the user's browser, executing before any communication happens with the ServiceNow server.

```text
User clicks UI Action
          ↓
Browser executes JavaScript
          ↓
Result displayed to user
```

Client-side UI Actions are typically used for showing alerts, confirmation messages, validating user input, changing form behavior, or asking for confirmation before an action proceeds. The VIP demo script above—`alert("VIP Button Clicked!");`—is a client-side example.

### Server-side UI Action

A **server-side** UI Action runs on the ServiceNow server and is used whenever the action needs to interact with the database.

```text
User clicks UI Action
          ↓
Request sent to ServiceNow server
          ↓
Server executes script
          ↓
Database updated
          ↓
Response returned to user
```

Server-side UI Actions are used for updating, creating, or deleting records, changing field values, running GlideRecord queries, or triggering backend operations. For example, a "Close Incident" button:

```text
current.state = 7;
current.update();
gs.addInfoMessage("Incident closed successfully");
```

```text
Click Close Incident
          ↓
State changes to Closed
          ↓
Record saved
          ↓
Message displayed
```

### Client-side vs Server-side — Comparison

| Client-side UI Action | Server-side UI Action |
|-------------------------|--------------------------|
| Runs in browser | Runs on ServiceNow server |
| Executes JavaScript locally | Executes backend logic |
| Faster response | Requires a server request |
| Cannot directly modify the database | Can modify the database |
| Uses `g_form` | Uses `GlideRecord` / `current` |
| Used for user interaction | Used for record processing |

### Combining Client-side and Server-side Logic

A single UI Action can use both—client-side validation followed by server-side processing—which is a common real-world pattern. For example, a "Close Incident after confirmation" requirement:

**Step 1 — Client-side confirmation:**

```text
function closeIncident(){
    if(confirm("Are you sure you want to close this incident?")){
        gsftSubmit(null, g_form.getFormElement(), 'close_incident');
    }
}
```

**Step 2 — Server-side update:**

```text
if(typeof window == 'undefined'){
    current.state = 7;
    current.update();
}
```

```text
User clicks Close Incident
            ↓
Client asks confirmation
            ↓
User confirms
            ↓
Submit to server
            ↓
Server updates record
            ↓
Incident closes
```

### Important UI Action Methods

| Method | Side | Purpose | Example |
|---------|------|---------|---------|
| `alert()` | Client | Displays a browser popup message | `alert("VIP Button Clicked!");` |
| `g_form.getValue()` | Client | Reads a field value from the form | `g_form.getValue('priority');` |
| `g_form.setValue()` | Client | Sets a field value on the form | `g_form.setValue('priority','1');` |
| `GlideRecord` | Server | Reads, creates, or updates database records | `var inc = new GlideRecord('incident'); inc.query();` |
| `gs.addInfoMessage()` | Server | Displays a message to the user after a server-side operation | `gs.addInfoMessage("Incident updated successfully");` |

---

## UI Action vs Client Script vs Business Rule

It helps to place UI Actions alongside two other automation tools that are frequently compared in interviews.

**UI Action vs Client Script** — both run on the client side, but serve different purposes:

| UI Action | Client Script |
|------------|----------------|
| Triggered by a button click | Triggered automatically by a form event |
| Requires user action | Requires a form event (load, change, submit) |
| Performs an operation | Controls form behavior |
| Example: Close Incident button | Example: Make a field mandatory |

**UI Action vs Business Rule** — one is user-triggered, the other runs automatically:

| UI Action | Business Rule |
|------------|-----------------|
| User-triggered | Automatically triggered |
| Requires user interaction | Runs on database operations (insert/update/delete/query) |
| Appears as a button/link/menu | Runs invisibly in the backend |
| Example: Approve Request button | Example: Auto-update assignment group |

### Real-World UI Action Examples

- **Close Incident** — sets State → Closed and Resolution → Completed.
- **Create Problem** — creates a Problem record and copies over relevant Incident details.
- **Assign VIP Support** — sets Priority → Critical, Assignment Group → VIP Support, and notifies the team.

---

## Interview Questions & FAQs

**What is a UI Action?**
A UI Action is a ServiceNow configuration used to create buttons, links, and context menu options that allow users to perform actions on records.

**Where can UI Actions appear?**
UI Actions can appear on Forms (as buttons or links), Related Links, Context Menus, and Lists (as banner buttons, bottom buttons, context menus, choices, or links).

**Can UI Actions be created on any table?**
Yes, UI Actions can be created on any table where customization is allowed.

**What controls where a UI Action appears?**
The display option checkboxes—Form Button, Form Link, Related Link, Context Menu, List Banner Button, and others—control exactly where the action shows up.

**Can UI Actions run on both client and server sides?**
Yes. UI Actions can execute client-side scripts, server-side scripts, or both, depending on the requirement.

**What is the difference between client-side and server-side UI Actions?**
Client-side UI Actions run in the browser and handle user interaction, while server-side UI Actions run on the ServiceNow server and handle database operations.

**Why do we use `alert()` in UI Action demos?**
The `alert()` function is a simple way to verify that a UI Action is triggering successfully when clicked, before adding real logic.

**Can UI Actions update records?**
Yes. Server-side UI Actions can update records using methods like `current.update()` or `GlideRecord`.

**What is the purpose of Show Insert and Show Update?**
Show Insert displays the UI Action while creating a new record; Show Update displays it while editing an existing record.

---

## Best Practices Learned

- Always double-check the **Table** field—a UI Action created on the wrong table simply won't appear where expected.
- Keep the **Active** checkbox and the relevant display-location checkboxes in mind together; a UI Action can be active but still invisible if no display option is enabled.
- Use `alert()` or `gs.addInfoMessage()` during development to confirm a UI Action is actually firing before adding real business logic.
- Prefer client-side confirmation (`confirm()`) before a destructive server-side action like closing or deleting a record.
- Keep client-side and server-side logic clearly separated within a script using the `typeof window == 'undefined'` check, so the same UI Action can safely handle both.
- Choose UI Actions for user-triggered operations, Client Scripts for form-event-driven behavior, and Business Rules for automatic database-level automation—picking the right tool avoids overlapping or conflicting logic.

---

## Memory Map

```text
UI Action
        │
        ├── Name / Table / Action Name
        │
        ├── Active / Show Insert / Show Update
        │
        ├── Display Locations
        │       ├── Form Button
        │       ├── Form Link
        │       ├── Related Link
        │       ├── Context Menu
        │       ├── List Banner Button
        │       ├── List Bottom Button
        │       ├── List Context Menu
        │       ├── List Choice
        │       └── List Link
        │
        ├── Script
        │       ├── Client-side (g_form, alert, confirm)
        │       └── Server-side (GlideRecord, current, gs)
        │
        └── Compared Against
                ├── Client Script (event-triggered)
                └── Business Rule (database-triggered)
```

### Quick Revision Sheet

```text
Create UI Action
All
└── System UI
    └── UI Actions
```

```text
Execution Flow
Create UI Action → Choose Table → Define Display Location → Add Script
    → User Clicks Action → Client/Server Logic Executes → Operation Completed
```

---

## Key Takeaways

- Understood UI Actions as configuration elements that let users trigger operations directly from a record via buttons, links, or menus.
- Learned the nine possible display locations a UI Action can appear in, on both forms and lists.
- Explored the key UI Action fields: Name, Table, Action Name, Active, Show Insert, Show Update, and Script.
- Built, configured, and tested a real **VIP** UI Action on the Incident table with every display option enabled.
- Learned the difference between client-side UI Actions (browser-executed, using `g_form` and `alert()`) and server-side UI Actions (server-executed, using `GlideRecord` and `current`).
- Understood how a single UI Action can combine client-side confirmation with server-side processing.
- Compared UI Actions against Client Scripts and Business Rules to understand when each tool is the right fit.

---

## Reflection

Day 08 completed the automation picture that started with Flow Designer on Day 07. Where Flows run silently in the background whenever a record changes, UI Actions put control directly in the user's hands—a button they click when they want something to happen right now. Building the VIP UI Action end-to-end, from an empty form to a working button appearing across the form, related links, context menu, and list view, made the configuration options feel far less abstract than reading about them.

The client-side versus server-side distinction was the most valuable concept from today. Understanding that a script can run purely in the browser for quick feedback, or reach into the database through `GlideRecord`, and that both can be combined in a single UI Action, explains a pattern I'll likely see again and again in real ServiceNow implementations—confirm first, then act on the server.

Comparing UI Actions to Client Scripts and Business Rules also helped sharpen my sense of when to reach for which tool: a button for something a user chooses to do, a Client Script for something that should happen automatically as a form is used, and a Business Rule for something that must happen no matter who or what touches the record.

```text
Day 07 → Flow Designer Automation (system-triggered)
             ↓
Day 08 → UI Actions (user-triggered)
```

Together, these two days now give me a working picture of both automatic and manual automation in ServiceNow. This repository will serve as my daily learning journal throughout the AICTE × ServiceNow Virtual Internship Program, documenting both theoretical concepts and practical exercises as I continue exploring the ServiceNow ecosystem.
