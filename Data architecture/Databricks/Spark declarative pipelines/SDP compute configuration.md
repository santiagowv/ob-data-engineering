![[Pasted image 20260228082825.png|573]]
We can use <span style="color:rgb(216, 203, 251)">serverless compute instead of configuring a cluster</span> with shared access mode.
# All purpose compute
Meant for <span style="color:rgb(216, 203, 251)">humans working interactively</span>. <span style="color:rgb(216, 203, 251)">Stays running</span> (until manually stopped or auto-terminated).
- <span style="color:rgb(216, 203, 251)">Developing</span> notebooks.
- <span style="color:rgb(216, 203, 251)">Ad-hoc</span> analysis
- Data <span style="color:rgb(216, 203, 251)">exploration</span>.
- <span style="color:rgb(216, 203, 251)">Debugging</span> pipelines.
- <span style="color:rgb(216, 203, 251)">Collaborative</span> work.
# Job compute
Meant for <span style="color:rgb(216, 203, 251)">automated, production workloads</span>. <span style="color:rgb(216, 203, 251)">Created automatically</span> when the job starts.
- <span style="color:rgb(216, 203, 251)">Scheduled ETL</span> jobs.
- DTL <span style="color:rgb(216, 203, 251)">pipelines</span>.
- Production <span style="color:rgb(216, 203, 251)">batch processing</span>.
- <span style="color:rgb(216, 203, 251)">CI/CD</span> execution.
- One-off <span style="color:rgb(216, 203, 251)">automated runs</span>.
## Serverless
Use serverless when we need a <span style="color:rgb(216, 203, 251)">fully managed platform requiring minimal additional configuration</span>:
## Classic
Use classic clusters <span style="color:rgb(216, 203, 251)">when we need additional control and configuration for our workloads</span>.
## Development vs production
- **Development:** <span style="color:rgb(216, 203, 251)">by default runs the job cluster for two hours</span> every time we run the pipeline.
- **Production:** by default <span style="color:rgb(216, 203, 251)">job clusters die automatically</span> when the pipeline finishes.