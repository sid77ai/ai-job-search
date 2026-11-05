# Firecrawl job providers - reference

Verified against the live Firecrawl Alexandria catalogue on 2026-11-05
(catalogue `sha256:b4329d9a…`). Contracts can change; re-read one with
`firecrawl_find_tools` (free) before using an option not listed here.

All calls: `firecrawl_scrape` with `alexandria: {provider, capability, options}`.
Cost: 5 credits per executed capability call (not per record). Discovery is free.

## indeed-com

| Capability | Required (one of) | Notes |
|---|---|---|
| `jobs/search` | `query`, `title`, `location` | Options: `country` (US, GB, CA, IN, DE, AU, NL, FR, NZ, IE, SG, ZA, ES, IT, MA, AE, MX, BR, JP, SE, CH, BE, AT, PH, MY, HK, EG), `radius_miles` (0-100), `posted_within_days` (1-30, listing date), `posted_at_within_days` (1-30, employer posting date), `exclude_reposts`, `job_type`, `remote`, `sort` (`relevance`/`date`), `limit` (1-100), `cursor`. Returns `jobs[]` + `next_cursor` (null = end). No total count. |
| `jobs/job` | `job_key`, `url` | `job_key` = 16 hex chars. Removed postings return `not_found`. |
| `jobs/employer` | `employer_key`, `company`, `url` | Employer research after search. |

Record fields (`jobs/search`): `job_key`, `title`, `employer{key,name,url}`,
`location{city,region_code,postal_code,formatted,...}`, `posted_at`, `listed_at`,
`reposted`, `expired`, `salary`, `snippet` (first 300 chars), `attributes[]`,
`language`, `source_name`, `source_url`, `apply_url`.

Live test (2026-11-05): `country: DE, query: "Director Supply Chain Transformation",
location: "Düsseldorf", radius_miles: 50, posted_within_days: 30, limit: 5` returned
5 records, `upstreamStatus: 200`, 5 credits.

## smartrecruiters-com

| Capability | Inputs | Notes |
|---|---|---|
| `jobs/search_jobs` | `query`, `company`, `location_type` (`remote`/`hybrid`/`onsite`) | Keyword or cross-company search. |
| `jobs/list_company_jobs` | company slug or pasted careers URL | Pass `next_cursor` back as `cursor` until null. |
| `jobs/get_job` | pasted posting URL, or company + id | Full text and salary. |

## ashbyhq-com

| Capability | Required (one of) | Notes |
|---|---|---|
| `jobs/jobs` | `board`, `url` | Options: `keyword` (title substring), `location` (substring), `department`, `team`, `employment_type`, `workplace_type`, `remote`, `page`, `per_page` (<=1000). |
| `jobs/job` | `url`, or `board` + `job_id` | Description, compensation, optional `application_form`. |

## Also in the catalogue (read the contract before first use)

`glassdoor-com/jobs/search_jobs`, `ziprecruiter-com/jobs/search_jobs` (US),
`workable/jobs/board_job`, `join-com/jobs/jobs` (DACH employer boards).
