# Day 02 — Lists, Records, Forms & ServiceNow Fundamentals
> **Date:** 10 July 2026
>
> Today's session focused on understanding how ServiceNow stores, displays, and manages data. We explored the relationship between records, fields, and values, learned the difference between List View and Form View, worked with filtering techniques, personalized lists, and gained an understanding of Formatters within forms.
---
# Table of Contents
1. Introduction
2. Record
3. Field
4. Value
5. List View
6. Form View
7. List View vs Form View
8. Formatter
9. Personalizing List Columns
10. Filtering Records
11. Applying Multiple Filters (AND)
12. Deleting an Incident
13. Incident as a Task Table
14. Key Learnings
---
# Introduction
Everything stored inside ServiceNow revolves around **tables** and the **records** they contain.
Whenever users create an Incident, User, Change Request, Problem, or any other object in ServiceNow, they are actually creating a **record** inside a specific table.
Understanding how records are displayed and managed is one of the first steps toward learning ServiceNow administration and development.
During today's session, we explored how ServiceNow organizes information and how users interact with it using **List View** and **Form View**.
---
# What is a Record?
A **Record** is a single entry stored inside a ServiceNow table.
Each record represents one object or piece of business information.
For example:
- One Incident
- One User
- One Department
- One Change Request
- One Problem
- One Knowledge Article
are all examples of records.
Think of a record as one complete row inside a database table.
---
## Example
Suppose the Incident table contains the following information.
| Incident Number | Caller | Priority |
|-----------------|---------|----------|
| INC0010001 | Beth Anglin | High |
| INC0010002 | Joe Employee | Low |
| INC0010003 | Abel Tuter | Medium |
Each row represents **one Incident record**.
---
# What is a Field?
A **Field** defines the type of information stored within a record.
Fields act as the columns of a table.
Each field stores one particular type of data.
Examples include:
- Number
- Caller
- State
- Priority
- Assignment Group
- Short Description
- Description
Every record contains values for these fields.
---
## Example
An Incident record may contain the following fields.
| Field | Value |
|--------|-------|
| Number | INC0010001 |
| Caller | Beth Anglin |
| State | New |
| Priority | High |
| Assignment Group | Hardware |
| Short Description | Printer not working |
Here,
- Number is a field.
- Caller is a field.
- Priority is a field.
---
# What is a Value?
A **Value** is the actual information stored inside a field.
For example,
| Field | Value |
|--------|-------|
| Caller | Beth Anglin |
| State | New |
| Priority | High |
"Beth Anglin", "New", and "High" are values.
Simply put,
- Record → Complete row
- Field → Column
- Value → Data inside a column
---
# Relationship Between Records, Fields and Values
```text
Incident Table
┌────────────────────────────────────────────┐
│ Record                                     │
│                                            │
│ Number : INC0010001                        │
│ Caller : Beth Anglin                       │
│ State : New                                │
│ Priority : High                            │
│ Assignment Group : Hardware                │
└────────────────────────────────────────────┘
Record
│
├── Field
│      └── Value
│
├── Field
│      └── Value
│
└── Field
       └── Value
```
Every record consists of multiple fields, and every field contains a corresponding value.
---
# List View
A **List View** displays multiple records simultaneously in a tabular format.
It is one of the most commonly used interfaces in ServiceNow because it allows users to quickly browse, search, sort, and filter records.
Instead of showing complete information about one record, List View focuses on displaying selected columns from many records.
---
## Characteristics of List View
- Displays multiple records.
- Shows selected columns only.
- Supports sorting.
- Supports filtering.
- Allows quick searching.
- Enables bulk operations.
- Can be personalized.
---
## Example
```text
Incident List
Number        Caller          Priority
INC001001     Beth            High
INC001002     Joe             Medium
INC001003     Abel            Low
```
In today's session, we observed approximately **98 Incident records** displayed together in a single List View.
---
# Form View
Unlike List View, **Form View** displays **only one record at a time**.
It provides complete details of that record and allows users to create, modify, or review its information.
Opening any record from a List View automatically switches to Form View.
---
## Characteristics
- Displays one record.
- Shows every available field.
- Allows editing.
- Supports Save, Update, Submit, Resolve, and Delete actions (depending on permissions).
- Organizes information into sections for easier navigation.
---
## Example
```text
Incident : INC001001
Caller : Beth Anglin
Priority : High
Assignment Group : Hardware
Short Description :
Printer is not responding.
Description :
Printer displays a paper jam error.
```
Instead of showing many incidents, the form focuses entirely on one Incident.
---
# List View vs Form View
| Feature | List View | Form View |
|----------|-----------|-----------|
| Records Displayed | Multiple | Single |
| Purpose | Browse records | View or edit one record |
| Layout | Table | Detailed form |
| Information | Selected columns | Complete record |
| Best Used For | Searching, filtering, sorting | Creating or updating records |
---
# Formatter
A **Formatter** is a special component displayed on a ServiceNow form.
Unlike normal fields, a formatter does **not** store data directly. Instead, it presents related information, actions, or timelines associated with the current record.
Formatters improve the usability of forms by displaying dynamic information in a structured way.
---
## Example: Activity Formatter
One of the most common formatters is the **Activity Formatter**.
It displays:
- Work Notes
- Additional Comments
- Journal Entries
- System Updates
- Record History
rather than acting as a normal field.
---
## Other Examples
Some commonly used formatters include:
- Activity
- Attachments
- Approval History
- Process Flow
- Related Records
- SLA Timeline
---
# Personalizing List Columns
ServiceNow allows users to customize the columns displayed in a List View.
During today's session, we learned how to remove unwanted columns from the Incident list.
---
## Steps
1. Open the Incident table.
2. Click the **Gear (⚙️)** icon.
3. Select **Personalize List Columns**.
4. Move fields between **Selected** and **Available** columns.
5. Save the changes.
As demonstrated, we hid the **State** field from the list.
This personalization affects only how the list is displayed—it does not delete the field from the table.
---
# Filtering Records
ServiceNow provides powerful filtering capabilities through the **Filter (Funnel)** icon.
Filtering helps users locate specific records from large datasets.
---
## Steps
1. Open the Incident table.
2. Click the **Filter (Funnel)** icon.
3. Select a field.
Example:
```
Caller
```
4. Choose an operator.
Example:
```
is
```
5. Enter a value.
Example:
```
Beth
```
6. Click **Run**.
ServiceNow displays only the Incident records where the **Caller** is **Beth**.
---
# Applying Multiple Filters (AND)
Filters can be combined using the **AND** operator.
This allows users to narrow down records even further.
Example:
| Field | Operator | Value |
|--------|----------|-------|
| Caller | is | Beth |
| Assignment Group | is | Hardware |
After clicking **Run**, the Incident list was reduced from approximately **98 records** to **2 matching records**.
This demonstrates how multiple conditions can significantly refine search results.
---
# Deleting an Incident
If a user has sufficient permissions, an Incident record can be deleted.
Steps:
1. Open the Incident record.
2. Open the **Additional Actions** menu.
3. Click **Delete**.
4. Confirm the deletion.
> **Note:** In production environments, deleting incidents is generally restricted. Organizations typically resolve or close incidents instead of permanently removing them.
---
# Incident is a Task Table
An important concept introduced today was that the **Incident** table extends the **Task** table.
This means Incident inherits common Task fields and functionality.
```text
Task
│
├── Incident
├── Problem
├── Change Request
└── Other Task-based Tables
```
Because of this inheritance, Incident automatically includes common fields such as:
- Number
- State
- Assigned To
- Assignment Group
- Priority
- Short Description
while also supporting Incident-specific fields.
---
# Key Learnings
By the end of today's session, I understood:
- The relationship between Records, Fields, and Values.
- The purpose of List View and Form View.
- How ServiceNow displays data in different ways.
- The role of Formatters within forms.
- How to personalize List View columns.
- How to filter records using one or multiple conditions.
- The difference between browsing records and editing a single record.
- That the Incident table is a child table of the Task table through table inheritance.
---


# Part 2 — Understanding Tables & Data Model in ServiceNow
> In this session, we explored one of the most fundamental concepts of the ServiceNow platform—**Tables**. Since ServiceNow is built on a relational database, almost everything stored in the platform exists as a record inside a table. We learned about different types of tables, how to create custom tables, default fields, Access Control Lists (ACLs), and how inheritance works through extensible tables.
---
# Table of Contents
1. What is a Table?
2. Types of Tables
3. Base Table
4. Core Table
5. Parent Table
6. Child Table
7. Custom Table
8. Task Table
9. Incident, Problem & Change Tables
10. Navigating to Tables
11. Creating a New Table
12. Label vs Name
13. Default Fields
14. Default ACLs
15. Creating Custom Records
16. Extensible Tables
17. Table Inheritance
18. Inherited Fields
19. Parent vs Child Records
20. Key Learnings
---
# What is a Table?
A **Table** is the primary structure used by ServiceNow to organize and store data.
Every application on the ServiceNow platform stores its information inside one or more tables. Each table contains records, and every record is made up of multiple fields.
Just as a spreadsheet contains rows and columns, a ServiceNow table contains records (rows) and fields (columns).
Examples of common tables include:
- User (`sys_user`)
- User Group (`sys_user_group`)
- Role (`sys_user_role`)
- Incident
- Problem
- Change Request
- Knowledge
- CMDB
---
## Database Structure
```text
Table
│
├── Record 1
├── Record 2
├── Record 3
└── ...
```
Each record contains several fields and values.
---
# Types of Tables
During today's session, we learned about different categories of tables used throughout the ServiceNow platform.
These include:
- Base Table
- Core Table
- Parent Table
- Child Table
- Custom Table
Understanding these relationships is important because ServiceNow relies heavily on table inheritance.
---
# Base Table
A **Base Table** is the highest-level table in a hierarchy.
It provides a common structure that other tables can inherit from.
Base tables generally contain fields and functionality shared across multiple applications.
---
## Characteristics
- Highest level in hierarchy.
- Contains common fields.
- Used for inheritance.
- Rarely used directly by end users.
---
# Core Table
A **Core Table** is a standard table provided by ServiceNow as part of the platform.
These tables support built-in applications and platform functionality.
Examples include:
- User
- Incident
- Task
- Problem
- Change Request
- Knowledge
Core tables are maintained by ServiceNow and form the backbone of the platform.
---
# Parent Table
A **Parent Table** is a table whose structure can be inherited by another table.
The parent defines the common fields that child tables automatically receive.
Example:
```text
Task
│
├── Incident
├── Problem
└── Change Request
```
Task acts as the Parent Table.
---
# Child Table
A **Child Table** inherits fields, properties, and functionality from its parent table.
The child can also introduce its own custom fields while continuing to use everything inherited from the parent.
Benefits include:
- Reuse existing structure.
- Reduce duplication.
- Maintain consistency.
- Simplify development.
---
# Custom Table
A **Custom Table** is created by administrators or developers to store organization-specific information.
Unlike standard ServiceNow tables, custom tables are designed according to business requirements.
Examples might include:
- Student Management
- Employee Assets
- Training Requests
- VIP Cohort
- Smartbridge
Custom tables can exist independently or extend another table.
---
# Task Table
The **Task** table is one of the most important tables in ServiceNow.
It serves as the parent for many process-based tables.
Several applications inherit from the Task table because they share common information such as assignment, state, priority, and descriptions.
---
## Common Task Fields
- Number
- State
- Priority
- Assigned To
- Assignment Group
- Short Description
- Description
- Created By
- Updated By
Rather than redefining these fields in every application, ServiceNow allows child tables to inherit them.
---
# Incident, Problem & Change Tables
Three of the most commonly used IT Service Management (ITSM) tables inherit from the Task table.
---
## Incident Table
Used for managing interruptions to IT services.
Examples include:
- Printer not working
- Laptop won't boot
- Email issues
- Network connectivity problems
---
## Problem Table
Used to identify and manage the root cause of recurring incidents.
Instead of fixing individual incidents repeatedly, a Problem record helps eliminate the underlying issue.
---
## Change Table
Used for planning and managing changes to IT infrastructure.
Examples include:
- Software upgrades
- Server maintenance
- Security patch deployment
- Network configuration updates
---
## Table Hierarchy
```text
Task
├── Incident
├── Problem
├── Change Request
```
Each child inherits common Task functionality while adding its own specialized fields.
---
# Navigating to Tables
To manage tables in ServiceNow:
1. Open the **Application Navigator**.
2. Navigate to:
```text
System Definition
        │
        ▼
      Tables
```
This opens a List View displaying all tables available within the current ServiceNow instance.
---
# Creating a New Table
During today's session, we created a new custom table.
---
## Steps
1. Navigate to:
```
System Definition → Tables
```
2. Click **New**.
3. Enter the **Label**.
Example:
```
VIP Cohort 6
```
4. Move the cursor outside the Label field.
ServiceNow automatically generates the internal **Name** field.
5. Save the table.
The new table is now created.
---
# Label vs Name
One important concept demonstrated today was the distinction between a table's **Label** and **Name**.
---
## Label
The Label is the display name shown to users.
Example:
```
VIP Cohort 6
```
---
## Name
The Name is the internal database identifier used by ServiceNow.
It is automatically generated after entering the Label.
Example:
```
u_vip_cohort_6
```
The Name is used internally by scripts, APIs, and database operations.
---
# Default Fields
Immediately after saving a newly created table, ServiceNow automatically initializes **six default system fields**.
Although these fields are initially empty, they provide the structure required for storing records and managing data within the table.
During today's demonstration, we observed that every newly created table automatically contained:
✅ **6 Default Fields**
These fields are managed by the platform and are available before any user records are created.
---
# Default Access Control Lists (ACLs)
Along with the default fields, ServiceNow automatically creates **four Access Control Lists (ACLs)**.
These security rules determine which users are allowed to perform specific operations on the table.
The four default ACLs are:
| ACL | Purpose |
|------|---------|
| Create | Create new records |
| Read | View records |
| Write | Update existing records |
| Delete | Remove records |
These ACLs provide an initial security model immediately after table creation.
---
# Creating Records in a New Table
Although the table now exists, it does **not** contain any records.
At this stage:
- The table has been created.
- Six default fields exist.
- Four ACLs exist.
- No records have been entered.
Records are added only after users create them.
---
# Extensible Tables
One of today's key concepts was the **Extensible** property.
A table must be marked as **Extensible** before other tables can inherit from it.
Without enabling this option, the table will not appear in the **Extends Table** dropdown.
---
## Enabling Extensible
1. Open the custom table.
2. Scroll to the table properties.
3. Enable **Extensible**.
4. Save the table.
The table can now serve as a parent table.
---
# Table Inheritance
ServiceNow supports object-oriented inheritance through tables.
Instead of recreating the same fields repeatedly, a child table can extend a parent table.
---
## Example
We created another table:
```
Smartbridge
```
Initially, **VIP Cohort 6** did not appear in the **Extends Table** list.
After enabling **Extensible**, it became available.
The Smartbridge table was then created by extending VIP Cohort 6.
---
## Inheritance Diagram
```text
VIP Cohort 6
│
├── Name
├── Branch
├── Default Fields
│
└── Smartbridge
      │
      ├── Name (Inherited)
      ├── Branch (Inherited)
      ├── Default Fields (Inherited)
      └── Additional Fields
```
---
# Inherited Fields
When a child table extends a parent table, all of the parent's fields become immediately available in the child.
There is no need to recreate these fields manually.
This greatly reduces duplication and promotes consistency across applications.
---
# Parent vs Child Records
Although fields are inherited, **records remain independent**.
Example:
```
VIP Cohort 6
Fields:
- Name
- Branch
Records:
Devpal
Rahul
Priya
```
```
Smartbridge
Fields:
- Name (Inherited)
- Branch (Inherited)
Records:
Aman
Neha
```
Notice that the records created in **Smartbridge** do not automatically appear in **VIP Cohort 6**, even though both tables share the same field structure.
This demonstrates one of the key principles of ServiceNow inheritance:
- ✅ Structure is inherited.
- ✅ Fields are shared.
- ❌ Records remain separate.
---
# Key Learnings
By the end of today's session, I understood:
- Tables are the foundation of data storage in ServiceNow.
- Every record belongs to a table.
- ServiceNow provides several types of tables, including Base, Core, Parent, Child, and Custom tables.
- The Task table acts as the parent for Incident, Problem, and Change Request tables.
- New tables are created through **System Definition → Tables**.
- ServiceNow automatically generates the internal table Name from the Label.
- Every new table contains **6 default fields** and **4 default ACLs**.
- A table must be marked as **Extensible** before other tables can inherit from it.
- Child tables inherit the parent's fields but maintain their own independent records.
---
# Part 3 — Working with Fields, Dictionaries & Record Management
> After understanding tables and inheritance, today's session moved to one of the most important aspects of ServiceNow administration—working with fields. We learned how to customize forms, create new fields, understand the Field Dictionary, configure Choice fields, and manage records effectively.
---
# Table of Contents
1. Understanding Fields
2. Default Fields vs Custom Fields
3. Configuring Form Layout
4. Creating a New Custom Field
5. Field Types
6. The Field Dictionary
7. Dictionary Entries
8. Choice Fields
9. Creating Choice Values
10. Personalizing Forms
11. Save vs Submit
12. Record Management
13. Key Learnings
---
# Understanding Fields
A **Field** is an individual attribute of a record.
Each field stores one specific piece of information about that record.
For example, an Incident record may contain:
| Field | Example Value |
|---------|---------------|
| Number | INC0010001 |
| Caller | Beth Anglin |
| State | New |
| Priority | High |
| Assignment Group | Hardware |
Every field has a **name**, a **data type**, and a **value**.
---
# Default Fields vs Custom Fields
ServiceNow automatically creates several default fields whenever a new table is created.
These fields are managed by the platform and are common across many tables.
However, organizations often require additional information that isn't included by default.
In such cases, administrators can create **Custom Fields**.
Examples:
Default Fields
- Created
- Updated
- Created By
- Updated By
Custom Fields
- Branch
- Employee Code
- College Name
- Student ID
- Batch
Custom fields allow organizations to tailor the platform to their own business requirements.
---
# Configuring Form Layout
Sometimes the required field already exists but isn't visible on the form.
Instead of creating a duplicate field, ServiceNow allows administrators to modify the layout of the form.
---
## Steps
1. Open any record.
2. Click the **Additional Actions (☰)** menu.
3. Select:
```text
Configure
↓
Form Layout
```
The Form Layout window displays two sections:
- Available Fields
- Selected Fields
Fields can be moved between these sections to customize what appears on the form.
---
# Creating a New Custom Field
If the required field does not already exist, it can be created directly from the Form Layout interface.
---
## Steps
1. Open **Configure → Form Layout**.
2. Click **Create New Field**.
3. Enter the field details.
Example:
```
Field Label
Name
```
4. Choose the appropriate field type.
5. Click **Save**.
The new field becomes part of the table.
---
# Automatically Generated Field Names
One interesting observation during today's session was how ServiceNow generates internal names for custom fields.
Suppose we create a field:
```
Name
```
Internally, ServiceNow stores it as:
```
u_name
```
The prefix:
```
u_
```
indicates that the field was created by the user (custom field).
This distinguishes it from platform fields.
Examples:
| Display Label | Internal Name |
|---------------|---------------|
| Name | u_name |
| Branch | u_branch |
| Student ID | u_student_id |
Whereas default fields use names such as:
| Display Label | Internal Name |
|---------------|---------------|
| Email | email |
| User ID | user_name |
| State | state |
This naming convention helps developers quickly identify custom fields.
---
# Field Types
Each field stores a specific type of data.
Common field types include:
- String
- Integer
- Boolean
- Date
- Date/Time
- Choice
- Reference
- Email
- URL
- Phone Number
- HTML
- Journal
- Currency
Choosing the correct field type ensures proper validation and user experience.
---
# The Field Dictionary
Every field in ServiceNow has an associated **Dictionary Entry**.
The Field Dictionary defines:
- Field Name
- Label
- Data Type
- Default Value
- Mandatory Status
- Read-only Status
- Attributes
- Dependencies
The Dictionary acts as the configuration layer for fields.
---
# Opening the Dictionary
To view a field's dictionary entry:
1. Open a record.
2. Right-click or access the field options.
3. Open the **Dictionary**.
The Dictionary provides complete configuration details for that field.
---
# Dictionary Entries
A Dictionary Entry controls the behavior of a field.
Examples of configurable properties include:
- Label
- Internal Name
- Data Type
- Maximum Length
- Mandatory
- Read Only
- Default Value
- Choice Type
- Attributes
- Dependency
Most advanced field customization begins within the Dictionary Entry.
---
# Choice Fields
A **Choice Field** presents users with a predefined list of options instead of allowing free-text input.
Example:
```
Branch
▼
Computer Science
Information Technology
Electronics
Mechanical
```
Choice fields help maintain consistent and validated data.
---
# Creating a Choice Field
During today's session, we created a custom field:
```
Branch
```
The field type selected was:
```
Choice
```
Initially, the dropdown contained no values.
The values needed to be created manually.
---
# Creating Choice Values
To populate a Choice field:
1. Save the table.
2. Click the field name.
3. Open the **Field Dictionary**.
4. Select:
```text
Configure Dictionary
↓
Choices
```
5. Click **New**.
For each choice, specify:
Example:
| Property | Value |
|----------|-------|
| Label | DS |
| Value | ds |
| Language | en |
| Inactive | False |
Repeat this process for additional options.
Example choices:
| Label | Value |
|--------|-------|
| DS | ds |
| AIML | aiml |
| CSE | cse |
| IT | it |
Once saved, these values appear automatically in the dropdown list.
---
# Record Management
After creating a table and defining its fields, records can be added.
Initially:
```
Fields
✓ Available
Records
✗ None
```
Once users begin creating entries, the table stores individual records.
Each record contains values for the fields defined in that table.
---
# Save vs Submit
Another important concept covered was the difference between **Save** and **Submit**.
Although both store data, their behavior differs.
---
## Save
When **Save** is clicked:
- The current record is saved.
- The user remains on the same form.
- Additional changes can still be made.
---
## Submit
When **Submit** is clicked:
- The record is created.
- The form closes.
- ServiceNow returns to the List View.
This is commonly used when creating new records.
---
# Why This Difference Matters
Suppose an administrator is creating a new User.
If additional information still needs to be entered later:
```
Save
```
is appropriate.
If the record is complete:
```
Submit
```
creates the record and returns to the list.
Understanding this distinction improves workflow efficiency.
---
# Best Practices
- Reuse existing fields whenever possible.
- Create custom fields only when necessary.
- Follow the `u_` naming convention for custom fields.
- Choose appropriate field types.
- Keep dropdown choices meaningful and standardized.
- Avoid duplicate fields.
- Use Choice fields instead of free-text where consistency is important.
---
# Key Learnings
By the end of this session, I understood:
- The difference between default and custom fields.
- How to configure Form Layout.
- How to create new custom fields.
- Why custom fields receive the `u_` prefix.
- The purpose of the Field Dictionary.
- How Dictionary Entries control field behavior.
- How to create Choice fields.
- How to populate Choice values through **Configure Dictionary → Choices**.
- The difference between **Save** and **Submit** while managing records.
---
# Part 4 — Field Dependencies & Dot Walking in ServiceNow
> In the final technical session of today's class, we explored how ServiceNow can create intelligent and dynamic forms using **Field Dependencies**. We also learned one of the most widely used concepts in ServiceNow development—**Dot Walking**, which enables access to data stored in related tables without duplicating information.
---
# Table of Contents
1. Dynamic Forms
2. Field Dependency
3. Why Field Dependency is Important
4. Country → State Example
5. Configuring Dependent Fields
6. Advanced Dictionary Configuration
7. Creating Dependent Choice Values
8. Dynamic vs Manual Choices
9. Dot Walking
10. Real-World Examples
11. Benefits of Dot Walking
12. Best Practices
13. Key Learnings
---
# Dynamic Forms
Modern applications should display only the information that is relevant to the user.
Instead of overwhelming users with every possible option, ServiceNow allows administrators to create **dynamic forms** that automatically change based on previous selections.
This behavior improves:
- User Experience
- Data Accuracy
- Faster Form Completion
- Reduced Errors
One of the most common ways to achieve this is through **Field Dependency**.
---
# What is Field Dependency?
A **Field Dependency** is a relationship where the values available in one field depend on the value selected in another field.
Instead of displaying every possible option, ServiceNow filters the available choices based on the user's previous selection.
This helps maintain data integrity while simplifying the user interface.
---
# Real-World Example
Imagine a form containing two dropdown fields:
```
Country
▼
India
USA
Canada
Australia
```
```
State
▼
?
```
Initially, the **State** field should not display every state from every country.
Instead, once the user selects:
```
Country = India
```
The State field should automatically update to display:
```
Maharashtra
Gujarat
Karnataka
Tamil Nadu
Delhi
```
If the Country changes to:
```
USA
```
The available choices become:
```
California
Texas
Florida
Washington
New York
```
This behavior is called **Field Dependency**.
---
# Why Use Field Dependencies?
Without dependencies, users would see hundreds of unrelated options.
Example:
```
California
Texas
Florida
Delhi
Maharashtra
Ontario
Sydney
```
This creates confusion and increases the chance of selecting incorrect data.
Using dependent fields ensures that users only see valid choices.
Benefits include:
- Cleaner forms
- Better user experience
- Improved data consistency
- Reduced manual validation
- Faster data entry
---
# Configuring Dependent Fields
During today's session, we learned how to configure field dependencies using the **Field Dictionary**.
---
## Steps
1. Open the table containing the fields.
2. Open the desired field.
3. Access its **Dictionary Entry**.
4. Click **Advanced View**.
5. Locate the following property:
```
Dependent Field
```
6. Enable:
```
Use Dependent Field
```
7. Select the controlling field.
Example:
```
Country
```
8. Save the Dictionary Entry.
The selected field is now configured as a dependent field.
---
# Advanced Dictionary Configuration
The **Advanced View** of the Dictionary Entry provides additional configuration options that are not visible in the standard view.
Among these options is the ability to define dependencies between fields.
This configuration instructs ServiceNow to filter available choices based on the selected value of another field.
---
# Creating Dependent Choice Values
After defining the dependency, the next step is to configure the individual choice values.
For example:
### Country
```
India
USA
Canada
```
### State
Each State record is associated with a specific Country.
Example:
| Country | State |
|----------|-------|
| India | Maharashtra |
| India | Karnataka |
| India | Gujarat |
| USA | California |
| USA | Texas |
| USA | Florida |
| Canada | Ontario |
| Canada | Quebec |
When a user selects **India**, ServiceNow displays only the states associated with India.
---
# Dynamic vs Manual Choices
One question that naturally arose during the session was whether these dependent values are generated automatically.
The answer is:
**No—not by default.**
For standard Choice fields, administrators manually create the available choices and configure their dependencies.
However, ServiceNow also supports more advanced techniques for generating dynamic options, such as:
- Reference Fields
- Reference Qualifiers
- Dynamic Reference Qualifiers
- Client Scripts
- Script Includes
These allow dropdown values to be populated dynamically based on data stored in other tables.
While today's session focused on manually configuring dependent Choice fields, these advanced methods are commonly used in enterprise applications.
---
# What is Dot Walking?
**Dot Walking** is a feature in ServiceNow that allows users and developers to access fields from a referenced record without manually opening the related table.
Rather than storing duplicate information, ServiceNow retrieves the required data through a **Reference Field**.
The name "Dot Walking" comes from the use of the dot (`.`) operator.
---
# Understanding Dot Walking
Suppose an Incident contains the field:
```
Caller
```
The Caller field references the **User (`sys_user`)** table.
Instead of opening the User record separately, ServiceNow allows direct access to its fields.
Examples include:
```
Caller.Email
Caller.Phone
Caller.Department
Caller.Manager
```
Each dot represents moving one level deeper into the related record.
---
# Dot Walking Diagram
```text
Incident Record
│
├── Caller (Reference)
│
▼
User Record
│
├── Email
├── Phone
├── Department
├── Manager
└── Location
```
---
# Multi-Level Dot Walking
Dot Walking is not limited to a single relationship.
It can continue across multiple referenced records.
Example:
```
Caller.Manager.Email
```
Flow:
```text
Incident
│
▼
Caller
│
▼
Manager
│
▼
Email
```
ServiceNow automatically retrieves the manager's email address without requiring manual navigation.
---
# Where is Dot Walking Used?
Dot Walking is one of the most frequently used concepts in ServiceNow.
It appears in:
- Forms
- List Layouts
- Reports
- Notifications
- Flow Designer
- Business Rules
- Client Scripts
- Script Includes
- Reference Qualifiers
- GlideRecord Scripts
Because many ServiceNow tables are connected through Reference fields, Dot Walking provides a simple way to retrieve related information.
---
# Real-World Example
Suppose an Incident contains:
```
Caller
↓
Beth Anglin
```
The User record contains:
```
Email
beth.anglin@example.com
Department
IT
Manager
John Smith
```
Using Dot Walking:
```
Caller.Email
→ beth.anglin@example.com
Caller.Department
→ IT
Caller.Manager
→ John Smith
```
The related information is accessed directly without opening the User record.
---
# Benefits of Dot Walking
- Eliminates duplicate data.
- Simplifies scripts.
- Reduces database queries.
- Improves readability.
- Makes reports more informative.
- Enables access to related records with minimal effort.
---
# Best Practices
- Use meaningful Reference fields.
- Avoid storing duplicate information.
- Configure field dependencies where appropriate.
- Keep dropdown lists concise and relevant.
- Use Dot Walking instead of unnecessary duplicate fields.
- Validate dependent fields during form design.
---
# Key Learnings
By the end of today's session, I understood:
- How dynamic forms improve user experience.
- The concept of Field Dependency.
- How to configure dependent fields through the Dictionary's Advanced View.
- Why dependent Choice fields improve data quality.
- That dependent values are typically configured manually for Choice fields, while more advanced dynamic behavior can be achieved using reference-based techniques.
- The concept of Dot Walking.
- How Dot Walking retrieves data from referenced records.
- Why Dot Walking is one of the most powerful and frequently used features in ServiceNow administration and development.
---
# Part 5 — Day 02 Summary, Reflections & Quick Revision
> Day 02 focused on understanding how ServiceNow organizes data through tables, records, fields, forms, and lists. The session gradually moved from basic navigation to platform customization by introducing custom tables, field configuration, table inheritance, field dependencies, and Dot Walking. These concepts form the foundation for ServiceNow administration and application development.
---
# Table of Contents
1. Session Overview
2. Navigation Paths Learned
3. Important Terminology
4. Practical Activities Performed
5. Key Concepts Learned
6. Best Practices
7. Interview Questions
8. Personal Reflection
9. Day 02 Summary
10. What's Next?
---
# Session Overview
Today's session covered several core platform concepts that are essential for working with ServiceNow. Rather than simply interacting with existing records, we explored how administrators can configure and customize the platform to meet organizational requirements.
The session began by understanding how data is represented through **Records, Fields, and Values**, followed by the differences between **List View** and **Form View**. We then explored the platform's data model by learning about **Tables**, their hierarchy, and inheritance.
The second half of the session focused on customization, including creating tables, configuring fields, working with choice lists, enabling table extensibility, implementing field dependencies, and understanding **Dot Walking**.
---
# Navigation Paths Learned
Throughout today's hands-on session, we navigated through several important areas of the ServiceNow platform.
## Viewing Tables
```text
Application Navigator
    ↓
System Definition
    ↓
Tables
```
---
## Configuring Form Layout
```text
Open Record
    ↓
Additional Actions (☰)
    ↓
Configure
    ↓
Form Layout
```
---
## Personalizing List Columns
```text
List View
    ↓
Gear Icon (⚙️)
    ↓
Personalize List
```
---
## Filtering Records
```text
List View
    ↓
Filter (Funnel) Icon
    ↓
Choose Field
    ↓
Operator
    ↓
Value
    ↓
Run
```
---
## Creating a Choice List
```text
Table
    ↓
Field
    ↓
Dictionary
    ↓
Configure Dictionary
    ↓
Choices
```
---
## Configuring Field Dependency
```text
Field Dictionary
    ↓
Advanced View
    ↓
Use Dependent Field
    ↓
Select Parent Field
```
---
# Important Terminology
| Term | Description |
|------|-------------|
| Record | A single row of data stored within a table. |
| Field | An attribute or column that stores information about a record. |
| Value | The actual data stored inside a field. |
| List View | Displays multiple records in a tabular format. |
| Form View | Displays a single record with all of its details. |
| Table | A database object that stores related records. |
| Parent Table | A table whose structure can be inherited by other tables. |
| Child Table | A table that inherits fields and behavior from a parent table. |
| Custom Table | A user-created table designed for specific business requirements. |
| Dictionary | Stores the configuration and properties of fields. |
| Choice Field | A field that presents a predefined list of selectable values. |
| ACL | Access Control List used to manage permissions. |
| Formatter | A UI component that displays related information such as activity, approvals, or attachments. |
| Field Dependency | Restricts available field values based on another field's selection. |
| Dot Walking | Accessing fields from related records through reference fields. |
---
# Practical Activities Performed
During today's session, the following practical tasks were demonstrated:
- Viewed Incident records in List View.
- Opened individual Incident records in Form View.
- Personalized List View by hiding the **State** column.
- Filtered records using the Filter (Funnel) icon.
- Applied multiple conditions using the **AND** operator.
- Learned how Incident records can be deleted (subject to permissions).
- Explored the relationship between the **Task** table and **Incident** table.
- Navigated to **System Definition → Tables**.
- Created a custom table (**VIP Cohort 6**).
- Understood the difference between **Label** and **Name**.
- Observed the creation of **6 default fields** and **4 default ACLs**.
- Created a custom field using **Configure → Form Layout**.
- Learned that custom fields receive the **`u_` prefix**.
- Created a **Choice** field and manually added Choice values.
- Enabled the **Extensible** property on a custom table.
- Created another table (**Smartbridge**) that extends **VIP Cohort 6**.
- Observed how inherited fields become available in child tables while records remain independent.
- Configured **Field Dependency** using the Field Dictionary.
- Learned the concept and practical use of **Dot Walking**.
---
# Key Concepts Learned
By the end of Day 02, I gained an understanding of:
- How ServiceNow stores information using tables and records.
- The relationship between records, fields, and values.
- The purpose of List View and Form View.
- The role of Formatters in displaying contextual information.
- Different categories of tables and how inheritance works.
- Creating custom tables and fields.
- The importance of ACLs for securing data.
- Configuring Choice fields and Choice values.
- The use of table extensibility for reusable data models.
- Implementing field dependencies to create dynamic forms.
- Using Dot Walking to access data from related records.
These concepts provide the foundation for future ServiceNow administration and development tasks.
---
# Best Practices
As a beginner, the following practices stood out as particularly important:
- Reuse existing tables and fields whenever possible.
- Create custom fields only when necessary.
- Follow the **`u_` naming convention** for custom fields.
- Use Choice fields to enforce standardized data entry.
- Configure dependent fields to improve user experience.
- Enable table extensibility only when inheritance is required.
- Avoid duplicating data by using Reference fields and Dot Walking.
- Apply ACLs to secure access to records and fields.
---
# Quick Revision Notes
## Remember:
- **Record** = One row of data.
- **Field** = One attribute of a record.
- **Value** = Data stored inside a field.
- **List View** = Multiple records.
- **Form View** = Single record.
- **Formatter** = Displays related information, not stored data.
- **Table** = Stores records.
- **Choice Field** = Dropdown with predefined values.
- **Dictionary** = Configuration of a field.
- **ACL** = Controls access permissions.
- **Dependent Field** = Filters values based on another field.
- **Dot Walking** = Accesses fields from referenced records.
---
# Interview Questions for Revision
### 1. What is the difference between a Record, Field, and Value?
### 2. What is the difference between List View and Form View?
### 3. What is a Formatter in ServiceNow?
### 4. What is the purpose of a Table?
### 5. Explain the difference between Parent and Child Tables.
### 6. What are Default ACLs?
### 7. Why do custom fields begin with the `u_` prefix?
### 8. What is the purpose of the Field Dictionary?
### 9. How do Choice fields work?
### 10. What is Field Dependency?
### 11. What is Dot Walking?
### 12. Why is table inheritance useful?
---
# Personal Reflection
Today's session significantly expanded my understanding of how the ServiceNow platform is structured internally. Rather than viewing ServiceNow as just an interface for managing incidents, I learned how the platform organizes data using tables, records, fields, and relationships.
The concepts of **table inheritance**, **field customization**, and **Dot Walking** were especially insightful, as they demonstrate how ServiceNow promotes scalability, reusability, and efficient data management. Understanding these fundamentals provides a strong foundation for exploring more advanced topics such as scripting, automation, Flow Designer, and application development in future sessions.
Documenting these learnings has also helped reinforce my understanding and will serve as a valuable reference throughout the internship.
---
# Day 02 Summary
Day 02 focused on the foundational architecture of the ServiceNow platform. I learned how information is organized using records, fields, and tables; how users interact with data through List and Form Views; and how administrators customize the platform by creating tables, fields, and choice lists. The session also introduced key concepts such as table inheritance, extensible tables, field dependencies, and Dot Walking, all of which are fundamental to building scalable and maintainable ServiceNow applications.
---
# What's Next?
As the internship progresses, I look forward to exploring more advanced ServiceNow topics, including:
- Users, Groups & Roles (Advanced)
- Access Control Lists (ACLs)
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
Each new topic will build upon the concepts learned during these foundational sessions.
---
> **End of Day 02** 🚀
*"Strong foundations lead to scalable solutions. Today's learning reinforced the core building blocks that power every ServiceNow application."*
