![[Pasted image 20260118201238.png]]
# Lambda architecture
We maintain <span style="color:rgb(216, 203, 251)">two paths</span>:
-  **Batch layer:** <span style="color:rgb(216, 203, 251)">Recomputes "truth" from all historical data</span> (accurate, consistent, but slower).
-  **Speed layer:** Processes new events in <span style="color:rgb(216, 203, 251)">real time</span> (fast, but often approximate).
## GCP implementation
### Speed layer
1. Producers: Pub/Sub.
2. Dataflow streaming job:
	- <span style="color:rgb(216, 203, 251)">Windowed aggregations</span>.
	- Write results to:
		- BigQuery (streaming inserts) for analytics.
		- Bigtable/Spanner/Redis for low-latency serving.
### Batch layer
1. <span style="color:rgb(216, 203, 251)">Raw data landed in GCS</span> (or BigQuery raw).
2. <span style="color:rgb(216, 203, 251)">Daily/hourly batch</span>:
	1. Dataflow batch, dataproc or BigQuery SQL.
3. Write cold/true <span style="color:rgb(216, 203, 251)">outputs to BigQuery</span> (partition tables).
### Serving layer
- A final <span style="color:rgb(216, 203, 251)">view in BigQuery</span>.
	- <span style="color:rgb(216, 203, 251)">Merge speed outputs with the latest batch snapshot</span>.
## When to use it
- We need <span style="color:rgb(216, 203, 251)">correctness guarantees via periodic recomputation</span>.
- Streaming outputs are "good enough now".
- Consumers need both <span style="color:rgb(216, 203, 251)">real time and historical accuracy</span>.
# Kappa architecture
We maintain <span style="color:rgb(216, 203, 251)">one path</span>:
- Everything is <span style="color:rgb(216, 203, 251)">treated as a stream</span>.
- If we need to recompute, <span style="color:rgb(216, 203, 251)">we replay the stream</span> (or reprocess from retained history) through the same pipeline.
- Less duplication, simpler codebase.
## GCP implementation
### Single streaming pipeline
1. Producers: Pub/Sub.
2. <span style="color:rgb(216, 203, 251)">Dataflow streaming</span> pipeline does:
	1. Parsing, validation, dedupe, enrichment.
3. Store the inmutable event log for replay:
	1. Pub/Sub retention is limited, so for true replay <span style="color:rgb(216, 203, 251)">we typically also land raw events to GCS</span>.
### Reprocessing/replay
<span style="color:rgb(216, 203, 251)">Re-run the same beam pipeline in batch mode</span> reading from GCS event archive.
## When to use it
- Less <span style="color:rgb(216, 203, 251)">maintenance</span>.
- Can afford <span style="color:rgb(216, 203, 251)">event retention for replay</span>.
- <span style="color:rgb(216, 203, 251)">Don't need separate speed vs batch</span> logic.