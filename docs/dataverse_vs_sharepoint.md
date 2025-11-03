Dataverse vs. SharePoint — Architecture Comparison for PRMS
This document compares Dataverse and SharePoint as data backends for the Project & Resource Management System (PRMS) solution. It highlights how the same Power Apps, Power Automate, and Power BI logic behaves across both environments.
________________________________________
🧩 Overview
Category	Dataverse	SharePoint
Licensing	Requires Power Apps Premium or Developer environment.	Included with Microsoft 365 (Standard licensing).
Data Capacity	Up to 10 GB per database (scalable via add-ons).	Limited by site storage (typically 1 TB per site).
Data Types	Rich relational model with lookups, choices, currency, autonumber, and polymorphic fields.	Simpler columns (text, choice, number, lookup). No relationships beyond basic lookups.
Performance	Optimized for Power Platform. Relational joins handled natively.	Slower for large datasets; delegation limits apply in Power Apps and Power BI.
Security	Role-based, row-level, and field-level security via Dataverse roles.	Item-level permissions; requires manual management and impacts performance at scale.
Integration Depth	Native integration with Power Apps, Power Automate, Copilot Studio, and Power BI.	Strong integration with Power Apps and Power Automate; weaker model for Power BI joins.
Offline Access (Mobile)	Supported (with sync).	Limited offline capabilities.
API Access	Full REST and OData APIs with metadata.	REST API exists but limited metadata and relational support.
Governance	Managed solutions, ALM pipelines, and environments.	Lacks formal ALM; version control handled manually.
Scalability	Suitable for enterprise, large datasets, multi-table joins.	Best for smaller organizations (<50k items per list).
Cost to Client	Higher upfront (Premium licensing) but enterprise-grade.	No extra cost under M365 E3/E5 licenses.
________________________________________
🏗️ Implementation Differences
1. Table/List Creation
•	Dataverse: Create tables in a solution. Use primary keys (GUIDs) and managed relationships.
•	SharePoint: Create lists manually or via PowerShell. Lookups mimic relationships but are limited to one level deep.
2. Forms and Apps
•	Dataverse: Model-driven app can be generated automatically from tables. Canvas app connects seamlessly with delegation.
•	SharePoint: Canvas apps only. Delegation warnings appear often — must use filters carefully (StartsWith, Filter with indexed columns).
3. Automation (Power Automate)
•	Dataverse: Triggers like When a row is added, modified, or deleted; can use rich filters and environment variables.
•	SharePoint: Triggers like When an item is created or modified; simpler expressions, slower for large volumes.
4. Security and Access Control
•	Dataverse: Assign roles (Employee, PM, Finance). Restrict by table or record.
•	SharePoint: Use list permissions. Enable item-level security so users can only see their items.
5. Power BI Connectivity
•	Dataverse: Use Dataverse connector or OData feed. Supports relationships and incremental refresh.
•	SharePoint: Use SharePoint Online List connector. Joins require merging queries in Power Query. No native relationships.
________________________________________
📈 Performance & Delegation
Operation	Dataverse	SharePoint
Large data (>100k rows)	Handles efficiently with native queries.	Risk of throttling; delegation limits at 2,000 records by default.
Cross-table lookups	Supported (many-to-one, one-to-many).	Single-level only.
Filter/Search	Delegable across multiple columns.	Must use indexed columns and delegable functions.
Real-time sync to Power BI	Near-real-time via Dataverse connector.	Delay up to several minutes (depends on refresh schedule).
________________________________________
🧮 Cost & Licensing Notes
Scenario	Recommended Backend
Enterprise, multi-department, ALM pipeline required	Dataverse
Small team using Microsoft 365 with no Premium licensing	SharePoint
Portfolio demo or freelance proof-of-concept	Both (side-by-side)
________________________________________
💡 Portfolio Demonstration Strategy
When showcasing this in interviews or client presentations: 1. Start with Dataverse (Premium path) — emphasize scalability, relationships, and managed solutions. 2. Switch to the SharePoint version — highlight accessibility and lower cost. 3. Show Power BI dashboards pulling from both datasets — proving functional parity. 4. Conclude with a slide/table like this doc to discuss trade-offs.
Example interview line: “I designed PRMS to operate identically on Dataverse and SharePoint, demonstrating how to balance scalability and accessibility depending on client licensing and data governance maturity.”
________________________________________
🔧 Future Enhancement Ideas
•	Auto-sync between Dataverse and SharePoint using Power Automate for hybrid environments.
•	Add Azure SQL path for enterprise scalability comparison.
•	Performance benchmarking dashboard (execution times, delegation counts, flow duration).
________________________________________
🏁 Summary
Dataverse = Power, Scale, Governance.
SharePoint = Accessibility, Familiarity, Low cost.
Supporting both within a single solution positions you as a versatile, enterprise-aware consultant who can meet any organization where they are.
