# Medicare Provider Utilization & Spending Analysis

A data engineering and analytics project on CMS Medicare public datasets — processing multi-million-row provider-level data to surface spending patterns, identify billing outliers, and produce a payer-style audit dashboard that flags provider behavior worth reviewing.

**Status:** Scoped · Build starts after Project 3 ships

---

## Why this project

Medicare is the largest healthcare payer in the United States and publishes some of the richest open healthcare datasets in the world. The **CMS Provider Utilization and Payment Data** covers tens of millions of rows detailing every provider, procedure, location, and payment amount — data that payers, researchers, and investigators use routinely for fraud / waste / abuse analysis and policy work.

Most healthcare portfolio projects use small, clean, synthetic datasets (UCI, Pima, Wisconsin). This project deliberately uses real, messy, large-scale public data — which is what a healthcare data engineer would actually encounter on the job.

## What this project does

- Ingests CMS Medicare Provider Utilization and Payment public data (multi-million rows)
- Processes it with PySpark into a normalized analytical layer
- Generates geographic, specialty, and procedure-level spending profiles
- Builds an anomaly detection layer to flag providers whose billing patterns deviate from their peer group — a common fraud/waste/abuse use case
- Publishes a Power BI dashboard for a hypothetical payer audit team, with a one-page executive summary in the repo

## Tech stack

| Layer | Tool | Why |
|---|---|---|
| Ingestion | Python downloaders + checksum validation | CMS files are large; reproducible ingestion matters |
| Processing | PySpark (local mode) | Right tool for multi-million-row aggregations |
| Storage | PostgreSQL (Supabase free tier) for aggregates; parquet for raw | Cost-appropriate; parquet for scale, Postgres for serving |
| Modeling | scikit-learn — isolation forest, peer-group z-scores | Interpretable anomaly detection |
| Visualization | Power BI | Standard in healthcare and payer organizations |

## Approach

1. **Ingestion** — pull CMS files with reproducible scripts, verify row counts and checksums
2. **Cleaning** — harmonize specialty taxonomies, geocode zip to HRR / state, handle NPI-level deduplication
3. **Segmentation** — group providers into peer cohorts (specialty × region × volume band)
4. **Anomaly detection** — per-cohort z-scores on billing amount, procedure mix, and beneficiary count; isolation forest as a cross-check
5. **Reporting** — Power BI dashboard with drill-down from geography → specialty → outlier provider list
6. **Writeup** — one-page exec summary plus methodology appendix

## Sample questions this answers

- Which Medicare specialties show the widest per-provider billing variance, and what drives it?
- Within a specialty-region peer group, which providers are the furthest outliers on billing per beneficiary?
- Do outlier patterns cluster geographically?
- How would a payer audit team prioritize a review queue from this data?

## Important caveats *(called out explicitly in the dashboard)*

- **Outliers are not fraud.** High billing can reflect case mix, sub-specialization, or severity. The project flags candidates for *review*, not accusations.
- **CMS data has known quirks.** Suppression rules for small cell counts, annual lags in publication, and taxonomy changes over time are all documented in the repo.
- **No PHI is involved.** CMS public datasets are de-identified and aggregated.

## Repository layout

```
.
├── ingestion/            # CMS file downloaders + validation
├── spark_jobs/           # PySpark processing pipelines
├── analysis/             # peer-group construction + anomaly detection
├── dashboards/           # Power BI files + screenshots
├── docs/                 # methodology + limitations writeup
└── README.md
```

## Roadmap

- [ ] v0.1 — ingestion of one year of provider utilization data
- [ ] v0.2 — PySpark cleaning + specialty/region normalization
- [ ] v0.3 — peer-group z-score outlier detection
- [ ] v0.4 — isolation forest cross-check + agreement analysis
- [ ] v0.5 — Power BI dashboard v1
- [ ] v1.0 — writeup published in `docs/`, dashboard screenshots in README

## About the author

Nithin Kilari — M.S. in Computer Science (Data Science), Oklahoma City University. Interested in data engineering and analytics applied to healthcare.

[LinkedIn](https://www.linkedin.com/in/kilari-nithin-619481272/) · [GitHub](https://github.com/NithinKilari)
