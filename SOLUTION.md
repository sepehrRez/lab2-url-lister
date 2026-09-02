# UrlCount - Execution and Performance Analysis

## Implementation Approach
I used paython for URLcounting for Hadoop implemetation.

- **Mapper**: I implemented URLMapper using Regex in Python.  Mapper reads the documents line by line and uses Regular expression (`href="([^"]*)"`) to extract all hyperlink references and outputs `<URL, 1>`.
- **Reducer**: Aggregates the occurrence count for each unique URL and applies a threshold filter, printing only URLs with a frequency strictly greater than 5 (`count > 5`).
- **`Makefile`**: Configured to bundle and ship `URLMapper.py` and `URLReducer.py` using Hadoop Streaming (`STREAM_JAR = /usr/lib/hadoop/hadoop-streaming.jar`).

## Execution Results & Benchmarking

| Cluster Setup | Execution Time | CPU Time Spent |
| :--- | :--- | :--- | :--- | :--- |
| **2 Workers** | 1m 39.061s | 15,440 ms | 
| **4 Workers** | 1m 50.332s | 36,780 ms | 

## Performance Analysis & Observation

Executing on 4 worker nodes was roughly 11 seconds slower than on 2 workers. This happened because the input files are very small (under 1 MB), so actual data processing took less than a second. Most of the runtime was just cluster startup and task coordination overhead. Adding more nodes didn't speed up the work—it just created more network communication and data shuffling between machines (`Shuffled Maps` increased from 30 to 154), which ended up slowing the job down.
