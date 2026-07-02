> **Sample document for the bootcamp.** Quick answers to the questions new SAS Viya users ask first. The full product PDFs in the collection are the authoritative source.

# SAS Viya — Getting Started FAQ

## The big picture
- **SAS Viya** is the analytics platform; you use it through web applications
  that share data and security.
- **SAS Studio** — write and run SAS code, and build no-code **Flows** for
  data preparation.
- **Model Studio** — build machine-learning **pipelines** (and automated
  pipelines) on a visual canvas.
- **SAS Visual Analytics (VA)** — explore data and author interactive
  **reports** and dashboards.
- **CAS (Cloud Analytic Services)** — the in-memory engine where analytical
  tables live. A **caslib** is a library of CAS tables (like a folder).

## Q: Where does my data live?
Two worlds: the **compute server** (classic SAS libraries like WORK — private
to your session) and **CAS** (in-memory, shared, fast). Analytics apps (VA,
Model Studio) read CAS tables, so data usually needs to be *loaded to CAS*.

## Q: How do I get a CSV into CAS?
In SAS Studio: **New → Import Data**, choose the file, pick the target caslib
(e.g. Public), run. Or in the Manage Data / Data Explorer app: **Import**,
drag the file in, choose the caslib. After loading, the table must be
*promoted* (global scope) for other sessions/apps to see it — the import
wizards do this by default.

## Q: What's a promoted table?
A CAS table with *global* scope: visible to all sessions and applications,
not just the session that created it. Session-scope tables disappear when the
session ends.

## Q: How do I build my first model?
Model Studio: **New Project** → pick the CAS table → choose the **target**
variable → use an automatically generated pipeline (AutoML) or drag nodes
onto the canvas → **Run**. The pipeline compares models and marks a
**champion**.

## Q: How do I make a report?
Visual Analytics: **New Report** → add a data source (a CAS table) → drag
objects (bar chart, list table, KPI) onto the canvas → assign data items to
roles. Save and share from the report menu.

## Q: Why can't I see my table in VA / Model Studio?
Usually one of: it's in WORK (compute) not CAS; it's session-scope (not
promoted); or it's in a caslib you don't have rights to. Check with the
Data Explorer.

## Q: What does "ERROR: File does not exist" / "table not found" usually mean?
The libref/caslib in the code doesn't match where the table actually is —
list the library first, confirm the exact name (CAS table names are
case-insensitive but must match), and check promotion/scope.
