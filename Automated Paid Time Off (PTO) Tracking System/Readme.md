# Automated Paid Time Off (PTO) Tracking System

## Overview

This project focuses on architecting and deploying an automated, end-to-end Paid Time Off (PTO) tracking system. The goal was to replace manual leave tracking by engineering a self-calculating relational database, a secure frontend submission portal, and a dual-engine notification system for asynchronous and real-time alerts. 

## Objectives

- Eliminate manual data entry and mathematical errors in leave balance calculations
- Design a secure, self-serve frontend portal for employee requests
- Implement external email routing (via Zapier) for batched manager notifications
- Configure internal database automations (via Airtable) for zero-latency employee status updates
- Establish a scalable relational database architecture utilizing primary/foreign keys, Rollups, and Formulas

## Environment & Tech Stack

- **Core Database & Logic**: Airtable
- **External Automation Engine**: Zapier
- **Communication Protocol**: SMTP / Gmail Integration
- **Architecture Pattern**: Relational Database Management System (RDBMS) & Event-Driven Automation

## Tools Used

- `Airtable` - For building the backend tables, relational links, logic formulas, frontend forms, and native automations.
- `Zapier` - For configuring polling-based Webhook triggers and dynamic payload mapping to external applications.

## Methodology

### Step 1: Architecting the Relational Database

I started by designing a 3-tier relational database structure in Airtable to act as the single source of truth for the organization's HR data. 

**The Managers Table:** Created to store the hierarchical approvers for the company, capturing the Manager's Name and Email Address.

![Figure 2: The Managers table establishing the organizational hierarchy.](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%202_The%20Managers%20table.jpg)
*Figure 2: The Managers table establishing the organizational hierarchy.*

**The Employees Table:** This serves as the core "anchor" of the system. I configured Lookup fields to pull the respective manager's email (Hop 1) and utilized Rollup fields combined with conditional filtering (`Status = Approved`) to automatically calculate "Used PTO". A secondary Formula field continuously calculates the "Remaining PTO".

![Figure 3: The Employees table showcasing the dynamic Rollup and Formula fields for PTO balances](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%203_The%20Employees%20table.png)
*Figure 3: The Employees table showcasing the dynamic Rollup and Formula fields for PTO balances.*

**The PTO Requests Table:** This transactional table logs every individual time-off request. I engineered a Formula field to calculate the exact number of working days between the `Start Date` and `End Date`. A secondary Lookup field isolates the Manager's Email to serve as the destination payload for the Zapier webhook.

![Figure 1: The main PTO Requests transactional table.](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%201_The%20main%20PTO%20Requests%20transactional%20table.jpg)
*Figure 1: The main PTO Requests transactional table.*

### Step 2: Designing the Self-Serve Employee Portal

To ensure data integrity, employees should not have raw access to the backend database. I designed a targeted frontend UI form that feeds directly into the `PTO Requests` table.

**Process:**
The form requires the employee to select their profile, input their requested dates, and provide a comment. Upon submission, the database creates a new record, calculates the requested days, and sets the default status to "Pending".

![Figure 9: Form filled and submitted by employee who requested for a time off.](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%209_Form%20filled%20and%20submitted%20by%20employee%20who%20requested%20for%20a%20time%20off.jpg)
*Figure 9: Form filled and submitted by employee who requested for a time off.*

### Step 3: Configuring the Manager Notification Engine (Zapier)

I utilized Zapier as the external automation engine to securely draft and route approval requests to the appropriate manager's corporate email.

**Why Zapier for this step:**
Using a 15-minute polling trigger in Zapier serves as a natural batching mechanism. Since PTO requests are rarely split-second emergencies, this prevents managers from being overwhelmed by instant pings.

**Configuration:**
1. **Trigger:** Airtable `New Record` in the `PTO Requests` table.
2. **Action:** Gmail `Send Email`.
3. **Payload:** The "To" address was dynamically mapped to the `Manager's Email` Lookup field. The body was constructed using dynamic variables (`[Employee Name]`, `[Start Date]`, `[End Date]`, `[Requested Days]`, and `[Employee's Comment]`).

![Figure 5: Zapier configuration mapping dynamic variables into the manager notification email](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%205_Zapier%20configuration%20mapping%20variables%20into%20the%20manager%20notification%20email.jpg)
*Figure 5: Zapier configuration mapping dynamic variables into the manager notification email.*

![Figure 6: The automated notification as received in the Manager's corporate inbox](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%206_The%20automated%20notification%20as%20received%20in%20the%20Manager's%20corporate%20inbox..jpg)
*Figure 6: The automated notification as received in the Manager's corporate inbox.*

### Step 4: Configuring the Employee Status Update Engine (Airtable Automations)

I engineered a secondary internal automation to handle the final decision routing.

**Why Airtable Automations for this step:**
Once a manager makes a decision (Approves or Declines), the employee requires immediate notification to finalize personal travel plans. Airtable's native automations execute with zero latency and preserve external Zapier task quotas.

**Configuration:**
1. **Trigger:** Airtable `When a record matches conditions` (Fires when `Status` changes to *Approved* or *Declined*).
2. **Action:** Airtable Native `Send Email`.
3. **Payload:** The email includes the updated status, the requested dates, and the specific notes left by the manager.

![Figure 7: Airtable's internal automation logic for instant employee updates.](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%207_Airtable's%20internal%20automation%20logic%20for%20instant%20employee%20updates.jpg)
*Figure 7: Airtable's internal automation logic for instant employee updates.*

![Figure 8: The zero-latency status update received by the employee upon approval](https://github.com/Fabelt14/AI-Automation/blob/main/Automated%20Paid%20Time%20Off%20(PTO)%20Tracking%20System/Images/Figure%208_The%20zero-latency%20status%20update%20received%20by%20the%20employee%20upon%20approval..jpg)
*Figure 8: The zero-latency status update received by the employee upon approval.*

## Future Improvements

To scale this application for enterprise use, the following features are slated for future deployment:

1. **Company Holidays Table:** Implement a secondary Airtable schedule to automatically deduct official company holidays from the `Requested Days` calculation.
2. **Half-Day Requests:** Add a boolean (checkbox) to the request form and adjust the Formula field to allow for 0.5 day deductions.
3. **Manager Dashboard (Airtable Interfaces):** Build a dedicated Interface specifically for managers to view and approve requests via a clean, restricted GUI without touching the raw database grid.
4. **Slack/Teams Integration:** Swap the Zapier Gmail action for a Slack/Teams Webhook, sending interactive approval buttons directly to a manager's direct messages.

[Click to see the Airtable View Here](https://airtable.com/app7u5KsPSHwWUoLj/shrVFhtRGTGZAMQO0)
---
*Built by **Asekun Fatai** — Open to roles in Business Automation, Workflow Engineering, and Operations.*
