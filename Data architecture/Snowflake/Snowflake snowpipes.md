Continuous data ingestion service designed to automatically <span style="color:rgb(216, 203, 251)">load data files into Snowflake tables as soon as the files become available</span> in an external stage.
![[Pasted image 20261121145612.png]]
- Enables <span style="color:rgb(216, 203, 251)">near real-time</span> data loading with minimal latency.
- <span style="color:rgb(216, 203, 251)">Eliminates the need to manually execute COPY</span> commands to load data.
- <span style="color:rgb(216, 203, 251)">Uses event notifications</span> to become aware of new file arrivals.
- It's recommended when <span style="color:rgb(216, 203, 251)">files can arrive irregularly or in-frequently and need to be immediately processed</span>.
- <span style="color:rgb(216, 203, 251)">Works best with a size range of 10 MB and 250 MB</span> otherwise we can ecounteer performance issues.
- It gets the compute needed to load files <span style="color:rgb(216, 203, 251)">using serverless compute</span>.
- Supports all file formats including semi-structured data.
# Snowpipes settings
- Pause a pipe: `ALTER PIPE <pipename> SET PIPE_EXECUTION_PAUSED=TRUE`
- Check status: `SELECT SYSTEM$PIPE_STATUS('<PipeName>')`
- `INFORMATION_SCHEMA.COPY_HISTORY`: <span style="color:rgb(216, 203, 251)">COPY commands</span> run by the pipe.
- `INFORMATION_SCHEMA.VALIDATE_PIPE_LOAD`: <span style="color:rgb(216, 203, 251)">row-level errors</span>, used for troubleshooting.
- `INFORMATION_SCHEMA.PIPE_USAGE_HISTORY`: <span style="color:rgb(216, 203, 251)">billing</span> and costs.
