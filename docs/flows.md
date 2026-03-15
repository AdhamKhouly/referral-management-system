# Power Automate Workflows

This document describes the automated workflows used in the Referral Management System.

The system uses Microsoft Power Automate to automate notifications, enforce business processes, and manage the referral lifecycle after candidate submission.

> Note: Because this project was developed inside a corporate environment, the actual flow packages are not included in the repository. This document provides a detailed description of each workflow instead.

---

## Workflow Architecture Overview

The automation layer connects SharePoint data updates with notifications and downstream processes.

The workflows support the following lifecycle:

1. A referral is submitted
2. Recruiters review the referral
3. A hiring decision is recorded
4. If hired, a probation period is tracked
5. After three months, HR is prompted to confirm probation completion
6. If successful, the referrer is notified to claim their referral reward

---

## 1. New Referral Notification

### Trigger

When a new item is created in the `Referrals_Table` SharePoint list.

### Purpose

Ensure all relevant stakeholders are immediately notified when a referral is submitted.

### Actions

The workflow sends notification emails to:

- the recruiter responsible for the job posting
- all users listed in the `Superusers` SharePoint list
- the employee who submitted the referral

### Email Content

The notification email includes:

- candidate name
- job title
- role ID
- level
- location
- referrer name

### Business Impact

This workflow ensures that recruiters and administrators are aware of new referrals immediately and can begin the review process without delay.

---

## 2. Referral Decision Notification

### Trigger

When the `Status` column in `Referrals_Table` is updated to:

- `Accepted`
- `Rejected`

### Purpose

Communicate the final outcome of the referral review process.

### Actions

The workflow sends emails to:

- the referrer
- the recruiter responsible for the role
- all superusers

### Email Content

The notification contains:

- candidate name
- job title
- role ID
- final decision status
- recruiter feedback (if provided)

### Business Impact

This automation ensures transparency in the referral process and keeps all stakeholders informed of the outcome.

---

## 3. Referrer Acceptance Notification

### Trigger

When the referral `Status` changes to **Accepted**.

### Purpose

Notify the referrer that the candidate they referred has been accepted.

### Actions

The workflow sends a notification email to the referrer confirming the successful outcome of their referral.

### Business Impact

This workflow reinforces engagement in the referral program and acknowledges successful contributions from employees.

---

## 4. Probation Reminder Workflow

### Trigger

When a `HireDate` value is assigned to a referral.

### Purpose

Track the probation period for hired candidates and prompt HR to confirm probation completion.

### Workflow Logic

1. A hire date is recorded in the referral record.
2. The workflow waits **three months**.
3. After the delay, reminder emails are sent to:
   - the recruiter responsible for the job posting
   - all superusers

### Email Content

The email contains:

- candidate name
- job title
- role ID
- hire date
- level
- location
- link to the HR Confirmation App

### Business Impact

This workflow ensures that probation completion is reviewed and prevents the referral reward process from being forgotten.

---

## 5. Referral Reward Notification

### Trigger

Triggered by the **HR Confirmation App** when probation completion is confirmed.

### Purpose

Notify the employee who submitted the referral that the candidate has successfully completed the probation period.

### Actions

The workflow sends an email to the referrer containing:

- candidate information
- role ID
- confirmation that probation was completed successfully
- instructions to contact HR and claim the referral reward

### Business Impact

This automation closes the referral lifecycle and ensures that referrers receive recognition and rewards for successful hires.

---

## Automation Design Principles

The workflow architecture was designed with the following principles:

- **Event-driven automation** using SharePoint list triggers
- **Minimal manual intervention** during the referral lifecycle
- **Clear communication between employees, recruiters, and administrators**
- **Time-based automation** for probation tracking
- **Separation of concerns** between referral management and reward confirmation

---

## Automation Lifecycle Summary

The complete referral lifecycle supported by automation is:

1. Referral submission
2. Stakeholder notification
3. Recruiter evaluation
4. Hiring decision notification
5. Hire date recording
6. Automated probation tracking
7. HR confirmation of probation completion
8. Reward notification to referrer

---

## Security and Privacy Considerations

To protect internal infrastructure and employee data:

- flow exports are not included in this repository
- no internal email addresses or company URLs are exposed
- only workflow logic and behavior are documented
- all sensitive information has been removed

The repository documents the **automation architecture**, not the production configuration.