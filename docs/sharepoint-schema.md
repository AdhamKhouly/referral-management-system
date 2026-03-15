# SharePoint Schema

This document describes the SharePoint backend structure used by the Referral Management System.

> Note: This repository documents the schema only. No production data, internal URLs, attachments, or confidential company records are included.

---

## Overview

The system relies on four SharePoint lists:

1. `Open_Roles`
2. `Referrals_Table`
3. `Recruiters_cafe`
4. `Superusers`

These lists support:

- job posting management
- employee referral submission
- recruiter and superuser access control
- referral lifecycle tracking
- downstream automation through Power Automate

---

## High-Level Data Relationships

The application uses logical relationships between lists, primarily through shared identifiers and email-based access control.

- `Open_Roles.Role ID` → `Referrals_Table.Role ID`
- `Open_Roles.Recruiter Email` → `Referrals_Table.Recruiter Email`
- `Recruiters_cafe.Emails` → recruiter access logic
- `Superusers.Email` → superuser access logic

---

## 1. Open_Roles

Stores all job postings managed in the Referral App.

### Purpose

This list is used to:

- display available roles in the **All Open Roles** tab
- support keyword search and department filtering
- allow superusers to create, edit, close, reopen, or delete postings
- identify the recruiter responsible for each job posting

### Columns

| Column Name | Type | Description |
|---|---|---|
| `Role ID` | Number | Unique numeric identifier for the role |
| `Job Title` | Single line of text | Title of the job posting |
| `Department` | Single line of text | Department associated with the role |
| `Status` | Choice | Role status: `open` or `closed` |
| `Location` | Single line of text | Role location |
| `Recruiter Email` | Single line of text | Email of the recruiter responsible for the role |
| `Description` | Multiple lines of text | Role description or notes |
| `Level` | Choice | Seniority or level of the role |
| `Modified` | Date and Time | System-managed modified timestamp |
| `Created` | Date and Time | System-managed created timestamp |
| `Created By` | Person or Group | System-managed creator |
| `Modified By` | Person or Group | System-managed last modifier |

### Notes

- Only roles marked as `open` are intended to appear to all employees in the browsing experience.
- The `Manage` tab allows superusers to toggle roles between `open` and `closed`.
- The list acts as the source of truth for available job postings.

---

## 2. Referrals_Table

Stores all submitted referrals and tracks each referral throughout its lifecycle.

### Purpose

This list is used to:

- save submitted referrals from employees
- associate referrals with a job posting through `Role ID`
- track recruiter actions and candidate progress
- store recruiter comments and hire date
- support automated notifications and probation follow-up logic

### Columns

| Column Name | Type | Description |
|---|---|---|
| `Job Title` | Single line of text | Title of the related job posting |
| `Referral Name` | Single line of text | Name of the referred candidate |
| `Referral Email` | Single line of text | Email address of the referred candidate |
| `Referrer Name` | Single line of text | Name of the employee who submitted the referral |
| `Referrer Personnel ID` | Number | Personnel ID of the referring employee |
| `Referrer Email` | Single line of text | Email address of the referring employee |
| `Status` | Choice | Referral status: `Accepted`, `Rejected`, `Shortlisted`, `Contacted`, or `Uncontacted` |
| `Role ID` | Number | Numeric identifier linking the referral to a role |
| `Recruiter Email` | Single line of text | Recruiter responsible for the associated role |
| `Level` | Choice | Job seniority level |
| `Location` | Single line of text | Job location |
| `Description` | Multiple lines of text | Referral notes or supporting details |
| `Recruiter Feedback` | Multiple lines of text | Internal recruiter/superuser comments |
| `HireDate` | Date and Time | Hire date of the referred candidate, if applicable |
| `ProbationEmailSent` | Number | Flag used in probation reminder logic |
| `NotifyReferrer` | Number | Flag used in notification workflow logic |
| `EmailSentToReferrer` | Number | Flag used to track whether an email has already been sent |
| `Modified` | Date and Time | System-managed modified timestamp |
| `Created` | Date and Time | System-managed created timestamp |
| `Created By` | Person or Group | System-managed creator |
| `Modified By` | Person or Group | System-managed last modifier |

### Notes

- This list powers the **Recruiter Hub**.
- The `Status` field is central to recruiter workflows and notification triggers.
- `HireDate` is used to support the 3-month probation reminder workflow.
- `Level` and `Location` are stored with the referral record to preserve job context at submission time.
- Numeric flag fields support automation and help avoid duplicate actions.

---

## 3. Recruiters_cafe

Stores the email addresses of recruiters.

### Purpose

This list is used to:

- identify users who should have recruiter-level access
- control access to the **Recruiter Hub**
- support role-based visibility in the Referral App

### Columns

| Column Name | Type | Description |
|---|---|---|
| `Emails` | Single line of text | Recruiter email address |

### Notes

- Users whose email exists in this list are treated as recruiters by the app.
- This list is referenced during access checks.

---

## 4. Superusers

Stores the email addresses of privileged users with full access to the system.

### Purpose

This list is used to:

- identify users with full application privileges
- grant access to the **Manage** tab
- grant elevated access in the **Recruiter Hub**
- support admin-level actions across the app

### Columns

| Column Name | Type | Description |
|---|---|---|
| `Email` | Single line of text | Superuser email address |

### Notes

- Users whose email exists in this list are treated as superusers.
- Superusers can manage postings and perform elevated recruiter actions.

---

## Access Usage by List

| List Name | Used By |
|---|---|
| `Open_Roles` | All employees, superusers, automated workflows |
| `Referrals_Table` | Recruiters, superusers, automated workflows |
| `Recruiters_cafe` | Permission logic |
| `Superusers` | Permission logic |

---

## Schema Design Rationale

The SharePoint structure was designed to support an end-to-end referral workflow:

1. superusers create and manage job postings
2. employees browse open roles and submit referrals
3. recruiters and superusers review referrals and update candidate status
4. hire date information supports post-hire probation tracking
5. automation workflows notify stakeholders throughout the process

Because this is an internal enterprise solution, the repository documents only the schema design and excludes any production data or confidential business content.