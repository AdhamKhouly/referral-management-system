# Permissions and Access Control

This document describes the role-based access model implemented in the Referral Management System.

The system uses SharePoint lists and user email validation to dynamically determine which features and sections of the application are available to each user.

---

## Overview

The system defines three user roles:

- **Employee**
- **Recruiter**
- **Superuser**

Permissions are determined dynamically based on the user's email address and the contents of two SharePoint lists:

- `Recruiters_cafe`
- `Superusers`

---

## Role Definitions

### Employee

Employees represent the default user group. Any authenticated user who is not listed as a recruiter or superuser is treated as an employee.

Employees can:

- browse open roles
- search for roles
- filter roles by department
- view job descriptions
- submit referrals
- upload candidate resumes

Employees cannot:

- modify job postings
- view all referrals
- change referral statuses
- access recruiter tools

Employees interact exclusively with the **All Open Roles** tab.

---

### Recruiter

Recruiters are users whose email address appears in the `Recruiters_cafe` SharePoint list.

Recruiters can access the **Recruiter Hub** and manage referral reviews.

Recruiters can:

- browse submitted referrals
- search referrals
- filter referrals by status
- update referral status
- record hire dates
- view referral details
- add comments visible to other recruiters and superusers

Recruiters cannot:

- create or modify job postings
- access administrative role management features

---

### Superuser

Superusers represent the highest privilege level in the system.

Superusers are identified by their presence in the `Superusers` SharePoint list.

Superusers can access all system features.

Superusers can:

- browse open roles
- submit referrals
- manage job postings
- create new job postings
- edit job postings
- delete job postings
- toggle role status between open and closed
- access the Recruiter Hub
- update referral status
- add recruiter comments
- record hire dates
- participate in referral decision workflows

Superusers effectively act as both **administrators and recruiters**.

---

## Application Sections and Access

The Referral App contains three primary sections.

| Section | Employees | Recruiters | Superusers |
|--------|-----------|------------|------------|
| All Open Roles | ✓ | ✓ | ✓ |
| Manage | ✗ | ✗ | ✓ |
| Recruiter Hub | ✗ | ✓ | ✓ |

---

## Access Control Implementation

Access logic is implemented using email-based checks against SharePoint lists.

### Superuser Check

The application first checks whether the user's email exists in the `Superusers` list.

If the user is a superuser:

- full privileges are granted
- administrative features become available

### Recruiter Check

If the user is not a superuser, the application checks whether the user's email exists in the `Recruiters_cafe` list.

If the user is a recruiter:

- recruiter-level features are enabled
- access to the Recruiter Hub is granted

### Default Employee Role

If the user is not found in either list:

- the user is treated as a standard employee
- only employee-facing functionality is available

---

## Access Logic Flow

The application evaluates user roles using the following logic:

1. Retrieve the current user's email address.
2. Check if the email exists in `Superusers`.
3. If true, grant superuser privileges.
4. Otherwise, check if the email exists in `Recruiters_cafe`.
5. If true, grant recruiter privileges.
6. Otherwise, treat the user as an employee.

---

## Security Considerations

The access control system was designed with the following principles:

- **Least privilege**: users only see functionality relevant to their role.
- **Dynamic role evaluation**: permissions update automatically when SharePoint lists change.
- **Separation of responsibilities**: employees, recruiters, and administrators have clearly defined capabilities.

Because permissions are stored in SharePoint lists, administrators can modify access levels without modifying the application code.

---

## Administrative Flexibility

Using SharePoint lists for role management allows administrators to:

- grant recruiter privileges by adding an email to `Recruiters_cafe`
- grant superuser privileges by adding an email to `Superusers`
- revoke access simply by removing a user from the relevant list

This approach keeps the system flexible and easy to maintain.

---

## Access Control Summary

The system implements a lightweight but effective role-based access model.

Access levels are determined dynamically through SharePoint-based role lists, ensuring that permissions can be managed centrally while maintaining a responsive user experience within the application.