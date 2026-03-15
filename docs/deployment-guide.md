# Deployment Guide

This document explains how to deploy the Referral Management System in a new Power Platform environment.

The system consists of:

- Two Power Apps applications
- Four SharePoint lists
- Several Power Automate workflows

The deployment process involves preparing the SharePoint backend, importing the Power Apps applications, and configuring the automation workflows.

---

# Prerequisites

Before deployment, ensure the following requirements are met:

- Access to Microsoft 365
- Access to SharePoint
- Access to Power Apps
- Access to Power Automate
- Permission to create SharePoint lists
- Permission to import Power Apps applications

---

# System Components

The system contains the following components:

## Applications

| Application | Purpose |
|-------------|--------|
| Referral App | Main application used by employees, recruiters, and superusers |
| HR Confirmation App | Used to confirm probation completion and trigger referral reward notification |

## SharePoint Lists

| List | Purpose |
|------|--------|
| Open_Roles | Stores job postings |
| Referrals_Table | Stores all submitted referrals |
| Recruiters_cafe | Stores recruiter email addresses |
| Superusers | Stores privileged admin users |

## Workflows

| Workflow | Purpose |
|----------|--------|
| New Referral Notification | Notifies stakeholders when a referral is submitted |
| Referral Decision Notification | Sends emails when a candidate is accepted or rejected |
| Probation Reminder | Reminds HR after the probation period |
| Referral Reward Notification | Notifies the referrer when probation is successfully completed |

---

# Step 1 — Create SharePoint Lists

Create the following lists in SharePoint.

## Open_Roles

Create a list named:

`Open_Roles`

Add the following columns:

| Column | Type |
|------|------|
| Role ID | Number |
| Job Title | Single line of text |
| Department | Single line of text |
| Status | Choice (open / closed) |
| Location | Single line of text |
| Recruiter Email | Single line of text |
| Description | Multiple lines of text |
| Level | Choice |
| Modified | Date and Time |
| Created | Date and Time |
| Created By | Person or Group |
| Modified By | Person or Group |

---

## Referrals_Table

Create a list named:

`Referrals_Table`

Add the following columns:

| Column | Type |
|------|------|
| Job Title | Single line of text |
| Referral Name | Single line of text |
| Referral Email | Single line of text |
| Referrer Name | Single line of text |
| Referrer Personnel ID | Number |
| Referrer Email | Single line of text |
| Status | Choice |
| Role ID | Number |
| Recruiter Email | Single line of text |
| Level | Choice |
| Location | Single line of text |
| Description | Multiple lines of text |
| Recruiter Feedback | Multiple lines of text |
| HireDate | Date and Time |
| ProbationEmailSent | Number |
| NotifyReferrer | Number |
| EmailSentToReferrer | Number |
| Modified | Date and Time |
| Created | Date and Time |
| Created By | Person or Group |
| Modified By | Person or Group |

---

## Recruiters_cafe

Create a list named:

`Recruiters_cafe`

Add the following column:

| Column | Type |
|------|------|
| Emails | Single line of text |

Add recruiter email addresses to this list.

---

## Superusers

Create a list named:

`Superusers`

Add the following column:

| Column | Type |
|------|------|
| Email | Single line of text |

Add the administrators of the system to this list.

---

# Step 2 — Import Power Apps Applications

Open Power Apps:

`https://make.powerapps.com`

Navigate to:

`Apps → Import canvas app`

Import the following files from the repository:

```text
powerapps/referral-app.msapp
powerapps/hr-confirmation-app.msapp
```

During import:

- connect the apps to the SharePoint site
- connect each app to the four lists created earlier

---

# Step 3 — Configure Data Connections

Inside each app:

1. Open **Data Sources**
2. Add a **SharePoint connection**
3. Connect the following lists:

```text
Open_Roles
Referrals_Table
Recruiters_cafe
Superusers
```

Ensure that:

- Role ID fields are correctly referenced
- Email fields match expected logic
- Status fields use the correct choice values
- Level and Location fields are correctly mapped in both role and referral records

---

# Step 4 — Configure Power Automate Workflows

Create the following workflows in Power Automate.

---

## New Referral Notification

Trigger:

`When an item is created`

Source list:

`Referrals_Table`

Actions:

- send email to recruiter
- send email to referrer
- send email to superusers

---

## Referral Decision Notification

Trigger:

`When an item is modified`

Condition:

`Status = Accepted OR Status = Rejected`

Actions:

- send decision email to referrer
- notify recruiters
- notify superusers

---

## Probation Reminder Workflow

Trigger:

`When HireDate is set`

Logic:

`Delay 3 months`

Actions:

- send reminder email to recruiter
- send reminder email to superusers
- include link to HR Confirmation App

---

## Referral Reward Notification

Trigger:

`HR Confirmation App action`

Actions:

- send reward notification email to referrer

---

# Step 5 — Configure Permissions

Populate the access lists:

```text
Recruiters_cafe
Superusers
```

Users listed in these lists automatically gain additional privileges in the application.

---

# Step 6 — Test the System

Run the following tests.

## Referral submission

1. Submit a referral
2. Verify referral appears in `Referrals_Table`
3. Confirm notification emails are sent

## Recruiter workflow

1. Change referral status
2. Verify notification emails are triggered

## Hiring workflow

1. Assign HireDate
2. Verify probation reminder flow is scheduled

## HR confirmation

1. Use the HR Confirmation App
2. Confirm reward notification email is sent

---

# Security Considerations

Before deploying to production:

- ensure SharePoint permissions are correctly configured
- verify that only authorized users can modify lists
- confirm that email notifications do not expose sensitive information
- validate role-based access logic

---

# Deployment Checklist

Before going live, confirm that:

- all SharePoint lists exist
- Power Apps applications are imported
- SharePoint connections are configured
- workflows are active
- recruiter and superuser lists are populated
- test referrals have been successfully processed

---

# Maintenance

Administrators can maintain the system by:

- adding or removing recruiters in `Recruiters_cafe`
- updating superusers in `Superusers`
- modifying job postings in `Open_Roles`
- reviewing referral records in `Referrals_Table`

No application changes are required to update access roles.