# RAG Knowledge Base — Documents per Agent

Verified, publicly downloadable PDFs to load into each agent's RAM collection.
SAS PDF URL pattern: `https://documentation.sas.com/api/collections/<collection>/<version>/docsets/<docset>/content/<docset>.pdf?locale=en`
HTML fallback for any docset: `https://documentation.sas.com/doc/en/<docset>/latest/titlepage.htm`

> Re-check liveness with `curl -I` before ingesting (URLs verified via search index, not live fetch).

## 1. Data Steward Agent (discovery / profiling / quality)
- SAS Information Catalog: User's Guide 2024.09 — https://documentation.sas.com/api/collections/infocatcdc/v_042/docsets/infocatug/content/infocatug.pdf?locale=en — asset discovery, automated profiling, semantic types.
- SAS Information Catalog: Administrator's Guide 2024.10 — https://documentation.sas.com/api/docsets/infocatag/v_038/content/infocatag.pdf?locale=en — discovery agents & ingestion config.
- SAS Data Explorer: User's Guide — https://documentation.sas.com/api/collections/dprepcdc/v_009/docsets/datahub/content/datahub.pdf?locale=en (newer HTML: https://documentation.sas.com/doc/en/dprepcdc/v_012/datahub/titlepage.htm) — import/load to CAS, profiles, sample data.
- SAS Data Quality 3.5: Getting Started — https://documentation.sas.com/api/collections/dqcdc/v_031/docsets/dqgs/content/dqgs.pdf?locale=en — QKB, standardization/matching/parsing concepts.

## 2. Data Engineering Agent (prep / joins / transforms)
- SAS Studio: User's Guide 2025.05 — https://go.documentation.sas.com/api/collections/webeditorcdc/v_055/docsets/webeditorug/content/webeditorug.pdf?locale=en — Flows, Join/Filter/Transform/Query steps (the Viya 4 ETL doc).
- SAS Cloud Analytic Services: Fundamentals — https://go.documentation.sas.com/api/collections/pgmsascdc/v_012/docsets/casfun/content/casfun.pdf?locale=en — CAS sessions, caslibs, table lifecycle.
- SAS CASL Programmer's Guide — https://documentation.sas.com/api/collections/pgmsascdc/v_042/docsets/caslpg/content/caslpg.pdf?locale=en — programmatic CAS data management.

## 3. Model Builder Agent (Model Studio / AutoML)
- SAS Viya: Machine Learning User's Guide 2025.06+ (Model Studio/VDMML) — https://documentation.sas.com/api/docsets/vdmmlug/v_012/content/vdmmlug.pdf?locale=en — pipelines, nodes, automated modeling.
- SAS Model Manager: User's Guide 2025.06+ — https://documentation.sas.com/api/collections/mdlmgrcdc/v_058/docsets/mdlmgrug/content/mdlmgrug.pdf?locale=en — register/compare/deploy/monitor.
- Exploring SAS Viya: Data Mining and Machine Learning (free e-book) — https://support.sas.com/content/dam/SAS/support/en/books/free-books/exploring-sas-viya-data-mining-machine-learning.pdf — beginner walkthroughs.

## 4. Insights & Reporting Agent (Visual Analytics)
- SAS VA: Designing Reports — https://documentation.sas.com/api/collections/vacdc/v_038/docsets/vareports/content/vareports.pdf?locale=en — objects, layouts, filters, actions.
- SAS VA: Working with Report Data — https://documentation.sas.com/api/collections/vacdc/v_029/docsets/vareportdata/content/vareportdata.pdf?locale=en — data items, calculated items, aggregations.
- SAS VA: Getting Started with Reports — https://documentation.sas.com/api/collections/vacdc/v_021/docsets/vareportsgs/content/vareportsgs.pdf?locale=en — onboarding guide.

## 5. Platform Guide Agent ("how do I..." on Viya)
- Introduction to SAS Viya Platform — https://documentation.sas.com/api/docsets/calintro/v_001/content/calintro.pdf?locale=en — components & applications orientation.
- SAS Viya Platform: Overview — https://documentation.sas.com/api/collections/sasadmincdc/v_045/docsets/viyaov/content/viyaov.pdf — architecture & capabilities.
- Getting Started with SAS Viya Platform Operations — https://documentation.sas.com/api/collections/sasadmincdc/v_066/docsets/itopscon/content/itopscon.pdf?locale=en — ops/admin entry point.
- (share) SAS Studio User's Guide 2025.05 — most end-user "how do I" questions land here.

## 6. Procurement Integrity Agent (NCGR use case)
- Government Tenders and Procurement Law, KSA (English, MoF) — https://www.mof.gov.sa/en/Knowledgecenter/newGovTendandProcLow/Documents/Government_Tenders_and_Procurement_Law.pdf — the governing law for everything Etimad/NCGR processes (Royal Decree M/128, 2019). Official-translation fallback: https://laws.boe.gov.sa/BoeLaws/Laws/LawDetails/c2c05ee1-201a-48de-91e7-a9a700f2d14f/2
- Executive Regulations of the GTPL (English) — https://www.mof.gov.sa/en/Knowledgecenter/newGovTendandProcLow/Documents/Executive%20Regulations.pdf — procedures, thresholds, evaluation rules.
- OECD Guidelines for Fighting Bid Rigging in Public Procurement (2025) — https://www.oecd.org/content/dam/oecd/en/publications/reports/2025/09/oecd-guidelines-for-fighting-bid-rigging-in-public-procurement-2025-update_127880ea/cbe05a56-en.pdf — bid-rigging red-flag detection list.
- World Bank Fraud & Corruption Awareness Handbook — https://documents1.worldbank.org/curated/en/309511468156866119/pdf/877290PUB0Frau00Box382147B00PUBLIC0.pdf — fraud schemes & red flags across the cycle. Short companion: https://documents1.worldbank.org/curated/en/223241573576857116/pdf/Warning-Signs-of-Fraud-and-Corruption-in-Procurement.pdf
- UNODC Guidebook on Anti-Corruption in Public Procurement — https://www.unodc.org/documents/corruption/Publications/2013/Guidebook_on_anti-corruption_in_public_procurement_and_the_management_of_public_finances.pdf — risk mapping pre-tender/tender/post-tender.
