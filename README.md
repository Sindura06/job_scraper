# job_scraper

U.S. State Government Jobs Intelligence Platform
End-to-End Project Roadmap
Project goal

Build a system that monitors official U.S. state government job portals, refreshes automatically, captures only postings from the last 24 hours, normalizes them into one schema, and ranks roles relevant to data science, analytics, and early-career candidates.

This should look like a data engineering + scraping + NLP + agentic systems project, not just a script.

Phase 1 — Foundation and Project Setup
Goal

Set up the full project structure, database, configuration system, Docker environment, and source registry.

What to build
Project folder structure
Docker setup
PostgreSQL database
.env config system
portal registry
base job schema
portal health tracking schema
scripts for DB initialization and seeding
Deliverables
docker-compose.yml
.env.example
requirements.txt
config/settings.py
config/portals.yaml
db/models/job.py
db/models/portal.py
scripts/init_db.py
scripts/seed_portals.py
Output of this phase

A working local environment with seeded state portals and a database ready for crawlers.

Phase 2 — Crawler Layer
Goal

Build the crawling system that can fetch listings from different state job portals.

What to build
base crawler interface
portal-specific adapters
raw HTML/JSON storage
request retry logic
crawl logs
crawl scheduler
Suggested crawler types
static page crawler using requests
dynamic page crawler using Playwright
Adapter examples
neogov_adapter.py
jobaps_adapter.py
custom_state_adapter.py
workday_like_adapter.py
Deliverables
src/crawlers/base_crawler.py
src/crawlers/neogov_adapter.py
src/crawlers/jobaps_adapter.py
src/crawlers/custom_adapter.py
src/crawlers/playwright_helper.py
src/storage/raw_store.py
src/logging/crawl_logger.py
Output of this phase

The system should successfully fetch raw listing pages from at least 3–5 states.

Phase 3 — Parsing and Extraction
Goal

Convert raw portal pages into structured job records.

What to extract
title
agency
department
state
location
posting date
closing date
salary
job type
experience requirement
description
qualifications
job ID
source URL
What to build
field extractors
parser validation checks
per-portal parsing rules
parse status tracking
raw-to-structured transformation
Deliverables
src/parsers/base_parser.py
src/parsers/neogov_parser.py
src/parsers/jobaps_parser.py
src/parsers/custom_parser.py
src/validation/parser_checks.py
Output of this phase

You should be able to convert raw pages into clean structured records and insert them into the database.

Phase 4 — Normalization and Deduplication
Goal

Standardize fields across all states and remove duplicate postings.

What to normalize
date formats
salary ranges
job types
department naming
location naming
experience ranges
title cleaning
Deduplication rules

Use combinations of:

job title
agency
state
source URL
content hash
posting date
Deliverables
src/normalize/date_normalizer.py
src/normalize/salary_normalizer.py
src/normalize/location_normalizer.py
src/dedupe/dedupe_engine.py
src/dedupe/hash_utils.py
Output of this phase

A unified job_postings table containing comparable records across different state portals.

Phase 5 — Last 24 Hours Refresh Logic
Goal

Ensure the platform only surfaces newly posted roles from the last 24 hours.

What to build
freshness filter
timezone-aware timestamp handling
"first seen" tracking
update detection
previous run comparison
Key logic

A job should be shown if:

it was posted in the last 24 hours, or
it was first discovered in the last 24 hours
Deliverables
src/filters/freshness_filter.py
src/jobs/delta_detector.py
src/jobs/posting_window.py
Output of this phase

The system now behaves like a real jobs watcher, not just a historical scraper.

Phase 6 — Agent Layer
Goal

Add multiple agents to make the system more intelligent and modular.

Recommended agents
1. Source Discovery Agent

Finds and verifies official state job portals.

2. Portal Classifier Agent

Detects what kind of portal a state is using.

3. Crawl Agent

Chooses the right crawler strategy and fetches pages.

4. Extraction Agent

Parses job pages into structured fields.

5. Validation Agent

Checks completeness, drift, duplicates, and broken fields.

6. Relevance Agent

Scores whether the role fits a DS graduate or early-career user.

7. Alert Agent

Sends only qualifying jobs from the last 24 hours.

Deliverables
src/agents/source_agent.py
src/agents/classifier_agent.py
src/agents/crawl_agent.py
src/agents/extract_agent.py
src/agents/validation_agent.py
src/agents/relevance_agent.py
src/agents/alert_agent.py
Output of this phase

Your project now becomes an agent-based pipeline, which is much stronger for portfolio and interview discussion.

Phase 7 — NLP and Job Intelligence
Goal

Add data science value on top of the scraped postings.

Features to add
job family classification
skill extraction
entry-level suitability score
experience estimation
keyword clustering
semantic similarity search
trend analysis by state and agency
Useful labels
Data Analyst
Data Scientist
Data Engineer
BI Analyst
Research Analyst
GIS Analyst
Policy Analyst
IT / not relevant
Deliverables
src/nlp/skill_extractor.py
src/nlp/job_classifier.py
src/nlp/experience_parser.py
src/nlp/relevance_ranker.py
src/analytics/trend_engine.py
Output of this phase

Now the platform is not just collecting jobs — it is analyzing them.

Phase 8 — API and Dashboard
Goal

Create a usable interface to search, filter, and monitor jobs.

Backend

Use FastAPI for:

jobs API
portal health API
trends API
alerts API
Frontend options
Streamlit for fast MVP
React for polished version
Dashboard features
jobs from last 24 hours
filter by state
filter by job family
filter by entry-level relevance
filter by salary
portal health status
posting trends
Deliverables
src/api/main.py
src/api/routes/jobs.py
src/api/routes/portals.py
src/api/routes/trends.py
dashboard/app.py
Output of this phase

A full working user-facing product.

Phase 9 — Scheduling and Automation
Goal

Run the system automatically at intervals.

What to build
scheduler
queueing or cron-based execution
per-state refresh frequency
retry handling
failure alerts
Options
APScheduler
Prefect
Airflow
GitHub Actions for lightweight runs
Docker cron jobs
Deliverables
src/scheduler/run_scheduler.py
src/scheduler/jobs.py
src/workflows/daily_refresh.py
Output of this phase

The platform refreshes automatically and continuously tracks new jobs.

Phase 10 — Monitoring and Reliability
Goal

Make the system production-style and resilient.

Metrics to track
crawl success rate
parser success rate
missing field rate
duplicate rate
jobs found per run
jobs found per state
layout drift detection
portal failures
Deliverables
src/monitoring/metrics.py
src/monitoring/drift_detector.py
src/monitoring/health_checks.py
Output of this phase

You can explain reliability, observability, and data quality in interviews.

Phase 11 — Deployment
Goal

Deploy the project so it can run continuously.

Options
local Docker deployment
AWS EC2
Render / Railway for API
Supabase or managed PostgreSQL
S3 for raw crawl storage
Deliverables
production .env
deployment docs
Docker production config
public dashboard or private hosted API
Output of this phase

A hosted end-to-end platform you can demo.

Phase 12 — Portfolio and Resume Packaging
Goal

Turn the project into something valuable for job applications.

What to prepare
GitHub README
system architecture diagram
sample screenshots
dashboard screenshots
data flow diagram
metrics from runs
limitations and future work
resume bullet points
Strong resume framing
Built an agent-based public-sector job intelligence platform that monitored official U.S. state government portals, normalized multi-source job postings, and surfaced new openings from the last 24 hours.
Designed adapter-based scraping pipelines for heterogeneous state career platforms and improved reliability through portal-specific parsers, validation checks, and refresh monitoring.
Developed NLP-based job classification and relevance scoring to rank early-career data roles by skills, experience level, and job family.
