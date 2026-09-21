<span style="color:rgb(216, 203, 251)">Optimized to create code in Databricks-supported programming languages</span>, frameworks, and dialects. Not intended to be a general-purpose programming assistant.
- Generates and runs code.
- Builds pipelines.
- Debugs errors.
- Works directly with Unity Catalog objects.
- AI-based autocomplete.
- Serverless code execution.
# Genie code agent mode
To get <span style="color:rgb(216, 203, 251)">direct conversational response without kicking in agent mode</span> we can use "Just explain this to me, don't run any code".
Genie can <span style="color:rgb(216, 203, 251)">help with most pipeline development tasks</span>:
- Data discovery.
- Pipeline <span style="color:rgb(216, 203, 251)">code edits</span>.
- <span style="color:rgb(216, 203, 251)">Pipeline execution</span>.
- Understanding and <span style="color:rgb(216, 203, 251)">improving pipeline behavior</span>.
- Author a new pipeline.
- Fix issues.
# Security
<span style="color:rgb(216, 203, 251)">It's governed by Unity Catalog permissions</span>, it can only access data and perform operations that we have permissions for.
# Attach files
Genie Code supports the following file types:
- .png
- .jpg
- .jpeg
- .gif
- .webp
- .pdf
- .csv
- .xlsx
- .xls
- .json
- .jsonl
# Compute
<span style="color:rgb(216, 203, 251)">Uses serverless compute</span> by default.
- If we're on a page that already has a compute resource selected, <span style="color:rgb(216, 203, 251)">Genie uses that compute instead</span>.
# Fork a conversation
Useful when we want to <span style="color:rgb(216, 203, 251)">try a different approach without losing the work we've already done</span>.
- To fork a conversation, click ![Fork icon.](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIGhlaWdodD0iMTYiIHZpZXdCb3g9IjAgMCAxNiAxNiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIGQ9Ik0yIDIuNzVDMiAxLjIzMTIyIDMuMjMxMjIgMCA0Ljc1IDBDNi4yNjg3OCAwIDcuNSAxLjIzMTIyIDcuNSAyLjc1QzcuNSA0LjAwODc4IDYuNjU0MjUgNS4wNzAwMiA1LjUgNS4zOTY0OFY2Ljc1SDkuMjVDMTAuNzY4OCA2Ljc1IDEyIDcuOTgxMjIgMTIgOS41VjkuNjAzNTJDMTMuMTU0MyA5LjkyOTk4IDE0IDEwLjk5MTIgMTQgMTIuMjVDMTQgMTMuNzY4OCAxMi43Njg4IDE1IDExLjI1IDE1QzkuNzMxMjIgMTUgOC41IDEzLjc2ODggOC41IDEyLjI1QzguNSAxMC45OTEyIDkuMzQ1NzUgOS45Mjk5OCAxMC41IDkuNjAzNTJWOS41QzEwLjUgOC44MDk2NCA5Ljk0MDM2IDguMjUgOS4yNSA4LjI1SDUuNVY5LjYwMzUyQzYuNjU0MjUgOS45Mjk5OCA3LjUgMTAuOTkxMiA3LjUgMTIuMjVDNy41IDEzLjc2ODggNi4yNjg3OCAxNSA0Ljc1IDE1QzMuMjMxMjIgMTUgMiAxMy43Njg4IDIgMTIuMjVDMiAxMC45OTEyIDIuODQ1NzUgOS45Mjk5OCA0IDkuNjAzNTJWNS4zOTY0OEMyLjg0NTc1IDUuMDcwMDIgMiA0LjAwODc4IDIgMi43NVpNNC43NSAxLjVDNC4wNTk2NCAxLjUgMy41IDIuMDU5NjQgMy41IDIuNzVDMy41IDMuNDQwMzYgNC4wNTk2NCA0IDQuNzUgNEM1LjQ0MDM2IDQgNiAzLjQ0MDM2IDYgMi43NUM2IDIuMDU5NjQgNS40NDAzNiAxLjUgNC43NSAxLjVaTTMuNSAxMi4yNUMzLjUgMTEuNTU5NiA0LjA1OTY0IDExIDQuNzUgMTFDNS40NDAzNiAxMSA2IDExLjU1OTYgNiAxMi4yNUM2IDEyLjk0MDQgNS40NDAzNiAxMy41IDQuNzUgMTMuNUM0LjA1OTY0IDEzLjUgMy41IDEyLjk0MDQgMy41IDEyLjI1Wk0xMCAxMi4yNUMxMCAxMS41NTk2IDEwLjU1OTYgMTEgMTEuMjUgMTFDMTEuOTQwNCAxMSAxMi41IDExLjU1OTYgMTIuNSAxMi4yNUMxMi41IDEyLjk0MDQgMTEuOTQwNCAxMy41IDExLjI1IDEzLjVDMTAuNTU5NiAxMy41IDEwIDEyLjk0MDQgMTAgMTIuMjVaIiBmaWxsPSIjNkY2RjZGIi8+Cjwvc3ZnPgo=) <span style="color:rgb(216, 203, 251)">Fork conversation from here</span> under the Genie Code message you want to fork from.
# Instructions
Instructions can include:
- Information about <span style="color:rgb(216, 203, 251)">code formatting preferences</span>.
- <span style="color:rgb(216, 203, 251)">Context</span> about our role.
- <span style="color:rgb(216, 203, 251)">Guidelines</span> to follow.
- Coding <span style="color:rgb(216, 203, 251)">conventions</span>.
- Response <span style="color:rgb(216, 203, 251)">standards</span>.
## Workspace instructions files
Genie code <span style="color:rgb(216, 203, 251)">automatically discovers and reads </span>`AGENTS.md` `CLAUDE.md` files in the workspace.
## Types of instructions
Genie Code generally prioritizes <span style="color:rgb(216, 203, 251)">workspace instructions over user instructions</span>.
# Agent skills
They <span style="color:rgb(216, 203, 251)">follow the open standard of Agent Skills</span>. Can include:
- Guidance.
- Best practices.
- Reusable code.
- Executable scripts.
## Best practices
- **Step-by-step instructions:** Clear procedural guidance.
- **Examples:** Sample inputs and expected outputs.
- **Edge cases:** Common varations and exceptions.
## Types of skills
- **Workspace skills:** <span style="color:rgb(216, 203, 251)">Available to everyone in the workspace</span>. Enforce organization-wide standards, <span style="color:rgb(216, 203, 251)">deliver consistent output across users</span>, or serve different teams.
	- **Workspace skills**: `Workspace/.assistant/skills/`
- **User skills:** Used for <span style="color:rgb(216, 203, 251)">personal preferences</span> such as preferred libraries, coding style, or role and background context.
	- **User skills**: `/Users/{username}/.assistant/skills/`
## Example
```
Workspace/.assistant/skills/
├── ml-workflows/
   ├── SKILL.md                # Workflow overview and best practices
   ├── training-patterns.md    # Standard ML training patterns
   └── scripts/
         └── model-deploy.py   # Model deployment automation

/Users/{username}/.assistant/skills/
├── personal-workflows/
   ├── SKILL.md                # Workflow overview and best practices
   ├── etl-patterns.md         # Personal ETL best practices
   ├── dashboard-templates.md   # Reusable dashboard patterns
   └── scripts/
         └── pipeline-setup.sh   # Environment setup scripts
```
# MCP Server connections
It supports:
- Native connectors for third-party data sources.
- Manually adding MCP servers.
# Commands

|Prompt text|What Genie Code does|
|---|---|
|`/`|Displays common commands|
|`/doc`|Comments the code in a diff view|
|`/explain`|Provides an explanation of the code in a cell|
|`/fix`|Proposes a fix to any code errors in a diff view|
|`/findTables`|Searches for relevant tables based on Unity Catalog metadata. To find features or feature tables, mention “features” or “feature tables” in the query. See [Find features and feature tables using Genie Code](https://docs.databricks.com/aws/en/machine-learning/feature-store/uc/ui-uc#features).|
|`/findQueries`|Searches for relevant queries based on Unity Catalog metadata.|
|`/optimize`|Suggests performance improvements for SQL queries.|
|`/prettify`|Formats SQL queries for readability.|
# Pricing
Pay-as-you-go model. There are no seats or per-user licenses. <span style="color:rgb(216, 203, 251)">We pay for consumption, metered in DBUs</span> based on the underlying LLM usage.
- <span style="color:rgb(216, 203, 251)">Each user gets 150 free DBUs</span> (around $10.50/month) per month for Genie usage.
- <span style="color:rgb(216, 203, 251)">Consumption varies</span> with things such as task complexity, number of agent steps, model usage, context size, and actions Genie Code performs.
- A short question or small SQL/code edit <span style="color:rgb(216, 203, 251)">may use only a fraction of a DBU</span>. A larger task such as inspecting multiple tables and creating Lakeflow pipelines can consume multiple DBUs because Genie may take several model/tool calls behind the scenes.
# Scheduled tasks
Each tasks <span style="color:rgb(216, 203, 251)">executes the prompt as a full Genie Code session</span> and produces a new chat.
## How scheduled runs work
- <span style="color:rgb(216, 203, 251)">Auto-approve is always on</span> for scheduled run.
- Each run <span style="color:rgb(216, 203, 251)">creates a new chat</span>.
- Runs <span style="color:rgb(216, 203, 251)">use our permissions</span>.
## Limitations
- <span style="color:rgb(216, 203, 251)">Cannot be shared</span> with other uses.
- <span style="color:rgb(216, 203, 251)">Count towards our Genie usage</span>.
# Models
Uses a compound AI system rather than a single large language model. <span style="color:rgb(216, 203, 251)">Combines AI models with retrieval, ranking, and personalization components</span> to interpret the questions.
## Level of effort
- **Auto (default, recommended):** provides the <span style="color:rgb(216, 203, 251)">highest quality</span> for any task.
- **Low:** lowers the cost for simpler tasks.