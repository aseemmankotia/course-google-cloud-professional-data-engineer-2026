# Chapter 6 Lab: BigQuery Storage Optimization

**Estimated time:** 25 minutes

## Exercise

Design walkthrough: given a 50TB events table queried daily by date range and filtered by customer_id, design the optimal partition (by date) and clustering (by customer_id) scheme, and decide between on-demand and flat-rate pricing based on query volume

## Key takeaway

BigQuery storage questions test pairing partitioning (coarse pruning) with clustering (fine pruning) correctly, and matching pricing model to query volume predictability.
