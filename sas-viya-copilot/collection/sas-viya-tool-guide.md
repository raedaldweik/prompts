> **Sample document for the bootcamp.** A task → tool cheat sheet to help the agent pick the right MCP tool quickly.

# SAS MCP — Task → Tool Guide (Sample)

The SAS MCP server has many tools. Pick the **smallest set** for the step you're on.

## Discover data
| Goal | Tool |
|------|------|
| List CAS servers | `list_cas_servers` |
| List caslibs | `list_caslibs` |
| List tables in a caslib | `list_castables` |
| Row/column counts, size | `get_castable_info` |
| Column names/types/labels | `get_castable_columns` |
| Sample rows | `get_castable_data` |

## Analyse & visualise
| Goal | Tool |
|------|------|
| Run SQL / PROC | `execute_sas_code` |
| Make a chart (UI renders it) | `render_chart` |
| Natural-language insights | `explain_data` |

## Load data
| Goal | Tool |
|------|------|
| Upload a CSV to CAS | `upload_data` |
| Promote a table to memory | `promote_table_to_memory` |
| Generate a synthetic/mock CAS table from a column spec | `generate_synthetic_data` (propose schema → confirm → generate) |
| Files service (list/upload/download) | `list_files`, `upload_file`, `download_file` |

## Build & use models
| Goal | Tool |
|------|------|
| List AutoML projects | `list_ml_projects` |
| Create an AutoML project | `create_ml_project` (set the target) |
| Train / run the pipeline | `run_ml_project` |
| List registered/published models | `list_registered_models`, `list_models_and_decisions` |
| Score data in real time | `score_data` |

## Long-running work (don't block)
| Goal | Tool |
|------|------|
| Submit an async job | `submit_batch_job` |
| Check job status | `get_job_status`, `list_jobs` |
| Get the log | `get_job_log` |
| Cancel | `cancel_job` |

## Data insights
| Goal | Tool |
|------|------|
| Natural-language insight into a column (drivers, outliers) | `explain_data` |

> SAS **Visual Analytics report** authoring (creating/editing/exporting saved VA
> reports) is **not** part of this server — it is handled by a separate
> reporting agent. Use `render_chart` for in-chat visualisations.

## Rules of thumb
- **Discover before you analyse** — never guess column names.
- Prefer the **specific** tool over raw `execute_sas_code` when one exists.
- Keep tool output small (few columns/rows) so it fits the context window.
- Use **batch** tools for heavy jobs; poll status instead of blocking.

## Note on scoped deployments (TOOL_GROUPS)
The same server image can be deployed with only a subset of these tools
(`TOOL_GROUPS` environment variable) — that is how the orchestrated
specialist agents are built. If a tool listed here is missing, you are a
scoped specialist: do the task with the tools you have or say which
specialist owns it.
