# Referral Management System (Power Platform)

A fully adaptive **Employee Referral Management System** built using **Microsoft Power Apps, SharePoint, and Power Automate**.

The system allows employees to refer candidates for open roles, enables recruiters to manage referrals, and automates the entire referral lifecycle including hiring decisions and post-hire probation tracking.

This project demonstrates how a complete internal HR workflow can be implemented using the **Microsoft Power Platform** with minimal infrastructure while maintaining scalability, automation, and role-based access control.

---

## All Open Roles Interface

<p align="center">
  <img src="screenshots/open-roles-light.png" alt="All Open Roles Light Mode" width="44%" />
  <img src="screenshots/open-roles-dark.png" alt="All Open Roles Dark Mode" width="44%" />
</p>

<p align="center"><em>Light Mode and Dark Mode views of the All Open Roles experience</em></p>

<p align="center">
Employees can browse open positions, search by keyword, filter by department, inspect role details, and submit referrals directly through the application.
</p>

---

# System Overview

The system manages the complete lifecycle of employee referrals:

1. Employees browse open roles
2. Employees submit referrals
3. Recruiters review candidates
4. Recruiters update referral status
5. If hired, the hire date is recorded
6. After three months, the system triggers a probation reminder
7. HR confirms probation completion
8. The referrer is notified to claim their referral reward

The system consists of two Power Apps applications:

- **Referral App** – main application used by employees, recruiters, and superusers
- **HR Confirmation App** – used to confirm successful completion of the probation period

---

# System Architecture

<p align="center">
  <img src="diagrams/system-architecture.png" alt="System Architecture" width="88%" />
</p>

The architecture is composed of four layers:

### Users
- Employees
- Recruiters
- Superusers

### Application Layer
- Referral App (Power Apps)
- HR Confirmation App (Power Apps)

### Automation Layer
- Power Automate workflows for notifications and probation tracking

### Data Layer
- SharePoint lists used as the system backend

---

# Core Features

## 1. Browse Open Roles

Employees can browse available job postings.

Features include:

- keyword search
- department filtering
- viewing job descriptions
- referral submission

## 2. Referral Submission

Employees can refer candidates directly through the application.

Submitted referrals include:

- candidate name
- candidate email
- role information
- referrer information
- supporting notes

Once submitted, automated workflows notify recruiters and administrators.

## 3. Recruiter Hub

Recruiters and superusers can manage the entire referral pipeline.

Capabilities include:

- browsing submitted referrals
- filtering by status
- updating referral status
- adding recruiter feedback
- recording hire dates
- reviewing candidate progress

## 4. Role Management (Superusers)

Superusers can manage job postings directly from the application.

Administrative actions include:

- creating new roles
- editing existing postings
- toggling roles between open and closed
- deleting job postings

## 5. Automated Referral Lifecycle

Power Automate workflows handle key lifecycle events:

- new referral notifications
- hiring decision notifications
- probation tracking
- referral reward notifications

This automation ensures that the referral process runs with minimal manual intervention.

## 6. Fully Adaptive Experience

Both applications are designed to work across:

- desktop
- tablet
- mobile

The Referral App also supports:

- light mode
- dark mode
- dynamic responsive layouts
- orientation handling for mobile usability

---

# Application Screenshots

## Manage Roles

<p align="center">
  <img src="screenshots/manage-tab-light.png" alt="Manage Tab Light Mode" width="44%" />
  <img src="screenshots/manage-tab-dark.png" alt="Manage Tab Dark Mode" width="44%" />
</p>

<p align="center"><em>Superusers can create, edit, close, reopen, and delete job postings</em></p>

---

## Recruiter Hub

<p align="center">
  <img src="screenshots/recruiter-hub-light.png" alt="Recruiter Hub Light Mode" width="44%" />
  <img src="screenshots/recruiter-hub-dark.png" alt="Recruiter Hub Dark Mode" width="44%" />
</p>

<p align="center"><em>Recruiters and superusers can manage referral review, status updates, hire dates, and internal comments</em></p>

---

## Referral Submission Form

<p align="center">
  <img src="screenshots/referral-form-light.png" alt="Referral Form Light Mode" width="44%" />
  <img src="screenshots/referral-form-dark.png" alt="Referral Form Dark Mode" width="44%" />
</p>

<p align="center"><em>Employees can submit candidate referrals directly through the application</em></p>

---

## HR Confirmation App

<p align="center">
  <img src="screenshots/hr-confirmation-app.png" alt="HR Confirmation App" width="60%" />
</p>

<p align="center"><em>HR confirms whether a hired candidate has successfully completed the probation period, which triggers the referral reward notification</em></p>

---

# SharePoint Backend

The system uses SharePoint as the primary data store.

### Lists Used

| List | Purpose |
|-----|------|
| `Open_Roles` | Stores job postings |
| `Referrals_Table` | Stores employee referrals |
| `Recruiters_cafe` | Stores recruiter email addresses |
| `Superusers` | Stores administrative users |

The schema is documented in detail in:

```text
docs/sharepoint-schema.md
```

---

# Automation Workflows

The system uses **Power Automate** for event-driven automation.

Key workflows include:

- New referral notification
- Referral decision notification
- Probation reminder (3 months after hire)
- Referral reward notification

Workflow documentation is available in:

```text
docs/flows.md
```

---

# Access Control

The system uses role-based access control based on user email.

### Roles

| Role | Permissions |
|----|----|
| Employee | Browse roles and submit referrals |
| Recruiter | Manage referrals |
| Superuser | Full system administration |

Access control is managed through SharePoint lists:

- `Recruiters_cafe`
- `Superusers`

Detailed documentation:

```text
docs/permissions-and-access.md
```

---

# Deployment

The system can be deployed in any Microsoft 365 environment.

Deployment involves:

1. Creating SharePoint lists
2. Importing Power Apps applications
3. Configuring data connections
4. Setting up Power Automate workflows
5. Populating access control lists

Full deployment instructions are available here:

```text
docs/deployment-guide.md
```

---

# Repository Structure

```text
referral-management-system
│
├── powerapps
│   ├── referral-app.msapp
│   └── hr-confirmation-app.msapp
│
├── docs
│   ├── sharepoint-schema.md
│   ├── flows.md
│   ├── permissions-and-access.md
│   └── deployment-guide.md
│
├── diagrams
│   └── system-architecture.png
│
├── screenshots
│   ├── open-roles-light.png
│   ├── open-roles-dark.png
│   ├── manage-tab-light.png
│   ├── manage-tab-dark.png
│   ├── recruiter-hub-light.png
│   ├── recruiter-hub-dark.png
│   ├── referral-form-light.png
│   ├── referral-form-dark.png
│   └── hr-confirmation-app.png
│
└── README.md
```

---

# Security and Privacy

This repository contains only:

- application packages
- schema documentation
- architecture diagrams
- sanitized screenshots

No confidential company data, internal URLs, attachments, or employee information is included.

---

# Technology Stack

- Microsoft Power Apps
- Microsoft Power Automate
- SharePoint Online
- Microsoft 365

---

# License

This project is licensed under the MIT License.

```text
See LICENSE file for details.
```

---

# Author

Developed independently as part of an internal automation initiative to improve employee referral workflows using the Microsoft Power Platform.