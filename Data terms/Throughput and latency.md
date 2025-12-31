# Throughput
<span style="color:rgb(216, 203, 251)">Amount of data</span> processed over a period of time.
- Often measured in <span style="color:rgb(216, 203, 251)">gigabytes per second</span> (GBps) or records per second.
- High throughput means the system can handle and process a<span style="color:rgb(216, 203, 251)"> large volume</span> of data efficiently.
- Example: a pipeline that ingests 2 GBps of data is considered to have high throughput.
# Latency
- Refers to the <span style="color:rgb(216, 203, 251)">delay</span> between the time data is <span style="color:rgb(216, 203, 251)">ingested</span> and when it becomes <span style="color:rgb(216, 203, 251)">available</span> for querying or further processing.
- Low latency means there is <span style="color:rgb(216, 203, 251)">minimal delay</span>, and data is quickly available for use (i.e., in near real-time).
- Example: A system where data ingested becomes queryable within a few milliseconds.
# Throughput vs Latency
- High throughput != low latency.
- High throughput + high latency:
	- A system can process large amounts of data but still take time to make data available for use.
- Low latency + Low throughput:
	- Low latency means data is available quickly, but the system may struggle to handle large volumes of data.