# Building-Metrics-Pipeline-with-Prometheus-and-Thanos
## The Problem

You run a Kubernetes platform with more than 100 microservices. Each one exposes a /metrics endpoint, and Prometheus scrapes them every 15 seconds. During an incident, you need the last 15 minutes of data; for capacity planning, you need the last 6 months. One Prometheus pod cannot do both. Its local storage is bound by the pod's disk, and after the retention window, old data is deleted. Giving it a bigger disk only delays the wall.

## The Fix

Keep Prometheus short-retention and replaceable, and push everything else to object storage with Thanos:

- A Thanos Sidecar next to each Prometheus uploads completed Time Series Database (TSDB) blocks to an Amazon S3 bucket and serves recent data.
- A Store Gateway reads the old blocks back from S3.
- Thanos Query merges recent and historical data, removes duplicates, and gives Grafana one PromQL endpoint.
- A Compactor merges, downsamples, and applies retention to the data in S3.

## What You Will Build

- **Task 1:** Explore the baseline, one Prometheus scraping the microservices
See the limits of a single Prometheus (short retention and capped history)
- **Task 2:** Create an S3 bucket and the Thanos object-store secret
- **Task 3:** Attach the Thanos sidecar and run two Prometheus replicas
- **Task 4:** Watch 2-minute blocks ship to S3
- **Task 5:** Deploy the Thanos query plane and query recent and historical data from one endpoint
- **Task 6:** Compare Thanos and Prometheus side by side in Grafana
- Prove high availability by killing a replica
