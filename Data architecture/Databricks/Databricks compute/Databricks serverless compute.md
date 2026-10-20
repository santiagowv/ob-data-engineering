On-demand, automatically <span style="color:rgb(216, 203, 251)">managed compute that scales based on our workload requirements</span>.
# Serverless compute for notebooks
In a workspace enabled for serverless compute <span style="color:rgb(216, 203, 251)">all users have access to serverless compute for notebooks</span>.
# Serverless overspend protection
To control long-running queries, <span style="color:rgb(216, 203, 251)">serverless notebooks have a default execution timeout</span> of 2.5 hours (9000 seconds).
# High memory serverless compute
We can configure the notebook to <span style="color:rgb(216, 203, 251)">use a higher memory size</span>. This memory size setting <span style="color:rgb(216, 203, 251)">increases the size of the REPL memory used</span> when running code in the notebook.
The available memory options are:
- **Standard:** 16 GB total memory.
- **High:** 32 GB total memory. It has a higer DBU emission rate than standard memory.
# Serverless usage policy
They allow an organization to <span style="color:rgb(216, 203, 251)">apply custom tags on serverless usage for granular billing attribution</span>.
# Serverless environments
They're lightweight, <span style="color:rgb(216, 203, 251)">fast-to-deploy compute settings</span> that do not require classic compute resources. It supports:
- **Managed compute:** <span style="color:rgb(216, 203, 251)">Databricks abstracts away infrastructure management</span>, so we don't have to manually choose and configure cluster types or sizes.
- **Base envionment selection:** we can <span style="color:rgb(216, 203, 251)">select a base environment</span> instead of a Databricks Runtime version.
- **Library management:** they don't support init scripts. Instead, <span style="color:rgb(216, 203, 251)">we manage dependencies and libraries directly in the environment</span>.
- **Easy workspace provisioning:** serverless workspaces (which use serverless environments) are <span style="color:rgb(216, 203, 251)">provisioned rapidly through the account console</span>.
## Select a base environment
The base environment selector includes the following options:
- **Standard:** default serverless base environment with <span style="color:rgb(216, 203, 251)">Databricks-provided libraries</span>.
- **ML:** base environment with the <span style="color:rgb(216, 203, 251)">Python and system packages from Databricks Runtime for Machine Learning pre-installed</span>.
- **AI:** AI-optimized base environment with <span style="color:rgb(216, 203, 251)">pre-installed machine learning (ML) libraries</span>.
- **Workspace environments:** compatible base environments <span style="color:rgb(216, 203, 251)">configured for our workspace by an administrator</span>.
## Configure environment for job tasks
<span style="color:rgb(216, 203, 251)">Each job task runs in an isolated environment</span> that includes a base environment and any additional libraries we specify. The base environment sets:
- Python and Scala runtime version. only <span style="color:rgb(216, 203, 251)">dependencies required for the task are installed at runtime</span>.
- Pre-installed libraries.
## Configure environment for notebooks in job tasks
<span style="color:rgb(216, 203, 251)">We can configure a job-level environment for a notebook task</span>. In that case, the environment settings and dependencies specified at the job level override those from the notebook.
- The environment defined in the notebook is <span style="color:rgb(216, 203, 251)">used when running it interactively</span>.