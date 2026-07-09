# Day 01 - Official Start of the AICTE × ServiceNow Virtual Internship Program (VIP)

**Date:** 9 July 2026

---

## Introduction

Today marked the official beginning of my technical learning journey in the AICTE × ServiceNow Virtual Internship Program (VIP).

Although the internship began on 8 July 2026 with the onboarding process, where we created our ServiceNow University and ServiceNow Developer accounts, today was the first day dedicated to understanding the ServiceNow platform and exploring its core concepts.

The session focused on building a strong foundation before moving towards advanced administration and development topics. Instead of directly jumping into scripting or automation, we first learned how ServiceNow organizes data, manages users, controls permissions, customizes forms, and structures applications.

---

## Foundation Recap (8 July 2026)

Before beginning the technical sessions, we completed the initial setup by:

- Creating a ServiceNow University account.
- Creating a ServiceNow Developer account.
- Setting up access to our personal developer instance.
- Getting familiar with the internship structure and learning resources.

We were also introduced to the learning material available on ServiceNow University. As part of the internship, our first assigned self-paced course is **"Welcome to ServiceNow" (approximately 3 hours and 15 minutes)**. Other courses, including the Micro Certification and ServiceNow Fundamentals On Demand, were briefly introduced but are not yet part of the current assignment.

---

## Platform Overview

The session started with an introduction to the ServiceNow platform.

I learned that ServiceNow is a cloud-based platform where almost everything revolves around **tables**, **records**, and **forms**. Every module stores its information inside database tables, and users interact with these records through forms and list views.

Some of the core tables introduced today included:

- `sys_user`
- `sys_user_group`
- `sys_user_role`

These tables form the foundation of user and access management within the platform.

---

## User Administration

We explored how administrators manage users inside ServiceNow.

Topics covered included:

- Creating new user records.
- Understanding existing user records.
- Creating user groups.
- Assigning users to groups.
- Assigning and modifying user roles.
- Resetting a user's password as a System Administrator.
- Understanding User Impersonation and how administrators can temporarily access the platform as another user for testing and troubleshooting purposes.

---

## Applications and Modules

Another important concept introduced today was the relationship between **Applications** and **Modules**.

I learned that:

- One application can contain multiple modules.
- Applications can be created based on business requirements.
- Modules provide navigation to specific tables, reports, forms, or other functionality.
- Roles can be assigned to both applications and modules to control visibility and access.

This helped me understand how ServiceNow organizes different business processes within a structured navigation hierarchy.

---

## Forms and Fields

One of the most interesting parts of today's session was learning how ServiceNow forms are customized.

We explored:

- What fields are.
- How each field stores specific information about a record.
- How to customize forms using:

```
Configure → Form Layout
```

Through Form Layout, we learned that we can:

- Add existing fields already available in the table.
- Create entirely new custom fields whenever required.

After creating a custom field, I noticed that ServiceNow automatically prefixes its database name with **`u_`**, making it easy to distinguish custom fields from out-of-the-box platform fields.

For example:

Standard fields:

- `user_name`
- `email`

Custom fields:

- `u_employee_code`
- `u_project_name`

Hovering over a field also displays useful metadata, including its internal database name.

---

## Save vs Submit

An important usability concept introduced today was the difference between **Save** and **Submit**.

- **Save** stores the current record while keeping the user on the same form, allowing further modifications.
- **Submit** creates or saves the record and completes the current action, allowing the workflow to continue.

Although this appears to be a small feature, understanding the difference is essential while working with records inside the platform.

---

## Introduction to the Incident Table

Towards the end of the session, we received a brief overview of the Incident table.

Some key observations included:

- Every Incident is stored as a record.
- Each record contains multiple fields.
- Opening an Incident displays its complete information through a form.
- Creating a new Incident automatically generates a unique Incident Number.
- Record numbers are automatically maintained by the platform.

We also received a brief introduction to:

- Impact
- Urgency
- Priority
- Data Lookup Rules

I learned that **Priority is determined based on the combination of Impact and Urgency**, allowing organizations to standardize how incidents are classified.

---

## Activity Formatter

The Incident form also contains an Activity section.

I learned that:

- The Activity area is displayed using a **Formatter**.
- It records work notes, comments, and updates made throughout the lifecycle of an incident.

We also explored the available actions on an Incident record.

- **Update** saves modifications made to an existing Incident.
- **Resolve** changes the Incident State to **Resolved** once the issue has been addressed.

---

## Key Takeaways

Today's session focused on understanding the building blocks of the ServiceNow platform rather than advanced development.

Some of the concepts I learned include:

- ServiceNow platform architecture
- Tables and records
- List View
- Form View
- User management
- Groups
- Roles
- User impersonation
- Password reset
- Applications
- Modules
- Role-based access
- Form customization
- Existing and custom fields
- Internal field naming (`u_`)
- Save vs Submit
- Incident records
- Automatic Incident Number generation
- Impact, Urgency and Priority
- Data Lookup Rules
- Activity Formatter
- Basic Incident lifecycle

---

## Reflection

Today laid the foundation for everything that follows in the internship.

Rather than learning isolated features, I began understanding how ServiceNow organizes information, manages users, structures applications, customizes forms, and controls access through roles.

Although these concepts seem basic, they form the backbone of the platform, and I'm looking forward to building on them in the upcoming sessions.

This repository will serve as my daily learning journal throughout the AICTE × ServiceNow Virtual Internship Program, documenting both theoretical concepts and practical exercises as I continue exploring the ServiceNow ecosystem.