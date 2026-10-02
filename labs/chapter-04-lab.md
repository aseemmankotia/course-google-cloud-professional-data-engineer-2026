# Chapter 4 Lab: Batch ETL, Dataproc, and Change Data Capture

**Estimated time:** 20 minutes

## Exercise

Design walkthrough: design a CDC pipeline replicating an on-prem MySQL OLTP database into BigQuery near-real-time using Datastream, plus a nightly ephemeral Dataproc batch job for heavy Spark transforms

## Key takeaway

Batch and CDC questions test whether you pick the purpose-built managed service (Datastream for CDC, ephemeral Dataproc for scheduled batch) over a more general but costlier or custom-built alternative.
