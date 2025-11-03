# PRMS — Progress Log

Use this file to track daily work. Add a new entry at the top each day.

> Tip: Keep entries short and specific. One commit per day with the same message summary works great.

---

## Template (copy/paste)

```md
### YYYY-MM-DD — Day X
**Path:** Dataverse | SharePoint | Both  
**Time Spent:** ~__ hrs  
**Commit/Tag:** `<your-commit-hash-or-tag>`

**Work Completed**
- [ ] Task 1
- [ ] Task 2

**Observations / Challenges**
-

**Next Steps (tomorrow)**
-
```

---

## Example Entry

### 2025-11-03 — Day 1
**Path:** Both  
**Time Spent:** ~2.5 hrs  
**Commit/Tag:** `day-01-env-setup`

**Work Completed**
- [x] Created Developer environment (Dataverse) and noted URL
- [x] Created SharePoint Team Site `PRMS-SharePoint-Dev`
- [x] Initialized GitHub repo with folders (`/apps`, `/flows`, `/data`, `/docs`, `/powerbi`, `/solution`)
- [x] Added ERD to `/powerbi/model_diagram.png`
- [x] Added docs: `dataverse_vs_sharepoint.md`, updated `README.md`

**Observations / Challenges**
- Skipped “sample apps/data” due to capacity warnings; not needed for PRMS
- Decided to use draw.io (free) instead of Visio for ERD

**Next Steps (tomorrow)**
- Create Dataverse tables and mirror SharePoint lists
- Prepare seed CSVs and load initial data

