---
title: Billing Methodology
sidebar_label: Billing Methodology
---
import Code from '../../src/common/code/'




Wisdom uniquely manages both web app data collection and data management ("data warehousing"). Wisdom provides "object storage" through Amazon Web Service's [S3 service](https://aws.amazon.com/s3/pricing/), and also provides SQL database access through Amazon Web Service's [RDS PostgreSQL service](https://aws.amazon.com/rds/postgresql/pricing/). 

Wisdom exposes data management configurations- you can define how long to store data for, at what access frequency, data durability level ("storage tiers"), and how much RAM + storage to provision your PostgreSQL database. To keep pricing as simple and transparent as possible while maintaning so much configurability, Wisdom charges customers 100% on top of what AWS bills us, and a data ingestion fee each month for web application page views recorded. In the early days of Wisdom, we made asumptions about what kind of configurations most customers would want. We found these assumptions to be frequently wrong. In particular, storage durability/availability, and data retention (weeks vs years) faced frequent push back. Now you can have it any way you want- with clear pricing statistics broken down by service, by day.


## Pricing Components

| Component                 | AWS Service    | Pricing Model      | Description                                                |
|---------------------------|----------------|--------------------|------------------------------------------------------------|
| Data Ingestion Processing | -              | Per Unit           | Page Views per group of 1 million Page Views ($199)        |
| Object Storage            | S3             | "Cost Plus" (+1x) | $0.0125/GiB/month depending on storage tier (S3 Standard - Infrequent Access). |
| PostgreSQL Database       | RDS PostgreSQL | "Cost Plus" (+1x) | Starting at $15 per month depending on RAM + GiB configured, and $0.127 per GB of SQL storage. |
_Note 1: Wisdom's primary data region is Ireland (AWS region eu-west-1)._

_Note 2: Bandwidth Fees - Data Transfer Out of Amazon S3 + RDS to a destination outside of AWS is $0.05 per GiB, or $0.02 per GiB to AWS regions. These fees are not usually materially noticeable, but could matter for data migration or replication tasks. For example, migrating out all your data outside of AWS could cost around $90 USD if you have 1TiB of data, or $20 within an AWS region outside of Ireland)._


## Estimating Cost
For planning purposes, it can be hard to get a ballpark cost estimate for many enterprise services. In general, Wisdom's rough median pricing (skewed to smaller SaaS companies) is around $279 per month. For the median user, recording/s3 storage costs are around $5 per month, PostgreSQL database at $35 per month, Apache Superset (visualization dashboard) at $30, and data ingestion page views accounting for around $199 per month. By default, recorded sessions are saved for 3 months. You may reduce or extend data retention indefinately. For example, with 12 months of data retention you can expect to pay 4x more in S3 costs (previously $5/month, now $20/month). Average session recording sizes are approximately 80 KB each- but vary by with each web app and certain configurations such as enabling/disabling various recording modules.



## Upgrading Your Database Instance Size
- Single Availability Zone


By Default, new accounts are assigned `db.t3.micro` PostgreSQL instances provisioned in a single availability zone for a monthly price of around $18 per month (~$3.50 for SQL storage) providing 1 GiB of RAM and 2 virtual CPUs. You may upgrade these database instances following the table below- just reach out to a member of the Wisdom team. Note the table below provides the AWS cost. For Wisdom's billing, add an additional 1x to cover Wisdom's "cost plus" cut. Keep in mind that backup snapshots, bandwidth, and per GB storage costs also apply, usually around ~15% of the base cost.


| EC2 Instance Type | Monthly Price | RAM | CPU Count |
|----------------|-----------|---------|----------|
| <br/> -- Burstable General Purpose | | |        |
| db.t3.micro    | $14.40    |   1 GiB |   2 vCPU |
| db.t3.small    | $28.08    |   2 GiB |   2 vCPU |
| db.t3.medium   | $56.16    |   4 GiB |   2 vCPU |
| db.t3.large    | $112.32   |   8 GiB |   2 vCPU |
| db.t3.xlarge   | $224.64   |  16 GiB |   4 vCPU |
| db.t3.2xlarge  | $449.28   |  32 GiB |   8 vCPU |
| <br/> -- General Purpose | |         |          |
| db.m5.large    | $141.84   |   8 GiB |   2 vCPU |
| db.m5.xlarge   | $283.68   |  16 GiB |   4 vCPU |
| db.m5.2xlarge  | $567.36   |  32 GiB |   8 vCPU |
| db.m5.4xlarge  | $1,134.72 |  64 GiB |  16 vCPU |
| <br/> -- Memory Optimized ||         |          |
| db.r5.large    | $201.60   |  16 GiB |   2 vCPU |
| db.r5.xlarge   | $403.20   |  32 GiB |   4 vCPU |
| db.r5.2xlarge  | $806.40   |  64 GiB |   8 vCPU |
| db.r5.4xlarge  | $1,612.80 | 128 GiB |  16 vCPU |


---

## Technical Definitions


### Page View
Wisdom charges for each page view recorded. A page view is defined by either triggering a Javascript "page change" event (sometimes called a virtual page view), or by the initialization of the Wisdom tracking script- for example when loading a web page for the first time.
