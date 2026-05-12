Readme · MD
Copy

# Reddit Content Ingestion Pipeline
 
Incremental data ingestion pipeline that collects Reddit posts and comments into a Delta Lake Bronze layer for downstream sentiment analysis.
 
## Stack
 
- **Databricks** / **Delta Lake** (Lakehouse architecture)
- **AsyncPRAW** (async Reddit API client)
- **PySpark** + **Pandas** for data transformation
- **Delta Table MERGE** for idempotent upserts (deduplication on URL)
## What it does
 
1. Searches Reddit (`/r/all`) for a configurable keyword using Lucene syntax
2. Fetches full comment trees for each submission
3. Writes results in batches to a Delta table with MERGE logic — safe for repeated runs without duplicating data
4. Handles pagination via Reddit's `after` cursor for large result sets
## Design decisions
 
- **Bronze layer pattern**: raw data lands as-is, preserving full fidelity for downstream transformations
- **Idempotent writes**: `MERGE` on URL ensures the pipeline can be re-run or resumed without side effects
- **Batch processing**: configurable batch size controls memory footprint and API rate limiting
- **Schema evolution ready**: Delta Lake's schema enforcement catches upstream API changes early
 
