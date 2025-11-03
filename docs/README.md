# Help Desk / Service Request System (Power Platform Portfolio)

**Author:** John Rolzhausen  
**Stack:** Power Apps (Canvas), Power Automate, Dataverse, Power BI, (optional) Power Pages

## 1. Overview
This project demonstrates an end-to-end business solution for handling service requests/tickets in a realistic consulting scenario. It covers intake, routing, notification, technician triage, SLA tracking, and analytics.

**Key Features**
- Dataverse data model (Tickets, Technicians, Categories, SLA policies, Comments)
- Automated routing, SLA calculations, notifications (Power Automate)
- Technician Canvas app (queue, ticket detail, comments, status updates)
- Power BI analytics (SLA compliance, backlog aging, workload)
- Optional: Power Pages for external submissions (trial/demo)

## 2. Architecture

```mermaid
flowchart LR
    subgraph External
      A[User (External)] -- Submit Request --> P[(Power Pages / Forms)]
    end

    subgraph Dataverse
      D1[(Ticket)]
      D2[(Technician)]
      D3[(Category)]
      D4[(SLA Policy)]
      D5[(TicketComment)]
    end

    subgraph Automate
      F1[Flow: New Ticket Acknowledge & Route]
      F2[Flow: SLA Monitoring]
      F3[Flow: Resolution & Survey]
    end

    subgraph Apps
      C1[Canvas App: Technician]
    end

    subgraph Analytics
      B1[Power BI Report]
    end

    P -->|Create| D1
    D1 --> F1 --> D1
    F2 -.-> D1
    F3 --> D5
    C1 <--> D1
    C1 <--> D5
    B1 --> D1
    B1 --> D2
    B1 --> D3
```

## 3. Data Model (Dataverse)
See **dataverse_schema.csv** in this repository for a detailed, import-friendly schema with columns, types, and notes.

**Core Tables**
- **Ticket**: header (title, description, priority, impact, category, SLA, status, timestamps)
- **Technician**: maps to SystemUser; skills and capacity
- **Category**: category/subcategory, default priority, route-to technician
- **SLA Policy**: response/resolve targets by priority
- **TicketComment**: internal/external timeline

## 4. Automation (Power Automate)
### 4.1 Flow: New Ticket – Acknowledge & Route
- **Trigger:** When a row is added in *Ticket* (Dataverse)
- **Actions:**
  - Set/confirm AutoNumber (TKT-{{yyyy}}-{{0000}})
  - Lookup *Category* → default priority & route-to technician
  - Compute **SLA Due** from *SLA Policy* (by Priority)
  - Send **Acknowledgement email** to requester (shared mailbox)
  - Send **Assignment email** to technician; optional Teams post

### 4.2 Flow: First Response SLA Monitoring
- **Trigger:** Recurrence (e.g., every 30 minutes)
- **Query:** Tickets with Status = *New* where now() > SLA FirstResponse
- **Action:** Email tech + CC lead; flag ticket

### 4.3 Flow: Resolution & Survey
- **Trigger:** On Ticket.Status change to *Resolved*
- **Actions:** Email resolution summary with survey link; if survey received → add *TicketComment*; auto-close after N days

## 5. Canvas App (Technician)
**Screens**
- **My Queue:** assigned/open tickets with filters
- **Ticket Detail:** edit, comment (internal/external), change status, reassign
- **Dispatch (optional):** Kanban by status/tech

**Key Formulas (examples)**
```powerfx
// My Queue filter
Filter(Ticket, 'Assigned To' = User().Email && Status in ["New","In Progress","Waiting on Customer"])

// Mark First Response
Patch(Ticket, ThisItem, {'First Response At': Now(), Status: "In Progress"})
```

## 6. Power BI Report
**Measures**
- Open Tickets
- Resolved Tickets
- SLA Met % (First Response)
- SLA Met % (Resolve)
- Average Time to First Response (hrs)
- Average Time to Resolve (hrs)

**Pages**
- Overview KPIs, trend
- SLA compliance by priority/technician
- Workload & aging buckets
- Category insights & CSAT (if using surveys)

## 7. Setup & Demo Data
- Import or manually enter sample data from **sample_tickets.csv**
- Create choice sets for Priority, Impact, Status, Channel
- Seed technicians & categories to enable routing logic

## 8. Future Enhancements
- AI Builder (auto-categorize; sentiment)
- Teams bot & Adaptive Cards
- RLS in Power BI for client views
- File storage integration (SharePoint) with attachment URLs

## 9. Screenshots & Walkthrough
Add screenshots of:
- Dataverse tables & sample records
- Flow run history & emails
- Canvas app screens
- Power BI dashboard

(Optional) Link a short demo video here.

---

**Author Notes**  
This is a non-production demo. Some components (e.g., Power Pages) may require trial or specific licensing if enabled.
