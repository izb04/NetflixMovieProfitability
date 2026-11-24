# Predicting Movie Box-Office Performance (2023–2025)

ISA 401 Final Project, Fall 2026, Team [number], Section [A or B]

<!--
How to use this template (delete this comment block once your team is set up):

1. One teammate clicks "Use this template" > "Create a new repository" on GitHub.
   Owner: that teammate. Name: something descriptive (for example, ohio-housing-affordability).
   Visibility: Public.
2. That teammate opens Settings > Collaborators and adds every other team member.
3. Every team member clones the new repository in GitHub Desktop (File > Clone repository)
   to M:\isa401\ and opens project.Rproj in RStudio.
4. Replace every [bracketed] placeholder below. Keep the headings: they follow the
   Project page on Canvas, and this README is the index of your final submission.

Never commit API keys. Keep them in a .Renviron file in this folder (already in .gitignore).
-->

## Submission

| Item | Link |
|---|---|
| Dashboard (Tableau Public or Power BI) | [link] |
| Recorded technical presentation (YouTube, unlisted) | [link] |
| This repository | [link] |

## Team

| Member | What I built | Portfolio project entry |
|---|---|---|
| [First Last] | [for example: the API pulls and the merge] | [https://USERNAME.github.io/portfolio/#...] |
| [First Last] | [ ] | [ ] |
| [First Last] | [ ] | [ ] |

Each member adds this project to their own portfolio site, in their own words, with a link
back to this repository and the dashboard, and updates the Skills section of their portfolio
and of their `business_intelligence` README with skills they used in this project.

## The Question

Which movie characteristics known before release best predict worldwide box-office gross per dollar of production budget?

This project examines theatrical movies released between 2023 and 2025 to understand how characteristics such as genre, production budget, runtime, and content rating relate to box-office performance. We measure performance using the box-office-to-budget ratio, calculated as reported worldwide theatrical gross divided by production budget.

Rather than relying on a prepackaged dataset, our team is building an original dataset by collecting and combining information from multiple real-world sources using R. We began with a 20-movie pilot sample consisting of 15 randomly selected movies and five intentionally selected movies to test our data collection process before expanding the analysis.

The ratio is a measure of box-office performance relative to production budget, not actual profitability, because it does not account for marketing, distribution costs, or other expenses.

## Data Sources

At least three real-world sources, collected with at least two methods (web scraping, API,
LLM extraction from text).

| Source | Link | Method | One row is | Code |
|---|---|---|---|---|
| [TMDb] | [	https://developer.themoviedb.org/docs] | [API] | [One theatrical movie] | `markdowns/01_tmbd.Rmd` |
| [name] | [url] | [ ] | [ ] | [ ] |
| [name] | [url] | [ ] | [ ] | [ ] |

## Pipeline

| Step | File | What it does |
|---|---|---|
| 0. Movie selection (Aliya) | `markdowns/00_movie_selection.Rmd` | Builds a TMDb candidate pool for 2023–2025, selects 15 random and 5 intentional movies, verifies U.S. theatrical releases, and creates the 20-movie master dataset. |
| 1. TMDb acquisition (Aliya) | `markdowns/01_tmdb.Rmd` | Uses the TMDb API to collect budgets, revenues, genres, runtimes, and theatrical release dates. Cleans missing financial data and creates `movie_tmdb.csv` and `movie_ids.csv`. |
| 2. Clean and transform | `markdowns/02_clean.Rmd` | [ ] |
| 3. Merge | `markdowns/03_merge.Rmd` | [ ] |
| 4. Validate and summarize | `markdowns/04_validate.Rmd` | [ ] |

Raw files go in `data/raw/`; the cleaned and merged files go in `data/processed/`.

## How to Reproduce


### Software Requirements

- R version: 4.6.2
- RStudio
- Required R packages: `tidyverse`, `httr2`, `jsonlite`, and `knitr`
- A TMDb API Read Access Token

### API Configuration

1. Clone this GitHub repository and open `project.Rproj` in RStudio.
2. Obtain a TMDb API Read Access Token from https://www.themoviedb.org/settings/api.
3. Create a `.Renviron` file in the project root containing:

   `TMDB_TOKEN=your_token_here`

4. Restart the R session.
5. Confirm the token is available by running `nzchar(Sys.getenv("TMDB_TOKEN"))`. The result should be `TRUE`.

**Security:** Never commit `.Renviron` or API credentials to GitHub. The `.gitignore` file excludes `.Renviron`.

### Run the Data Pipeline

Run the following R Markdown files in this order:

1. `markdowns/00_movie_selection.Rmd`
   - Retrieves the TMDb candidate pool for 2023–2025.
   - Selects 15 random and 5 intentional pilot movies.
   - Validates U.S. theatrical release records.
   - Creates `data/processed/movie_master.csv`.

2. `markdowns/01_tmdb.Rmd`
   - Reads the 20-movie master dataset.
   - Retrieves movie financial and descriptive information using the TMDb API.
   - Cleans missing financial values and validates release dates.
   - Creates:
     - `data/processed/movie_tmdb.csv`
     - `data/processed/movie_ids.csv`
     - `data/processed/financial_coverage_summary.csv`

Additional acquisition, merging, and validation steps will be documented by the other team members.

### Pilot Data Quality

The initial pilot includes 20 movies: 15 randomly selected and 5 intentionally selected.

- 15 of 20 movies (75%) have both positive reported budget and revenue values.
- The random sample has 66.7% financial coverage.
- The intentional sample has 100% financial coverage.
- Missing financial values are retained as `NA` rather than treated as actual zeros.
- All 20 movies remain in the dataset for transparency.

The project uses the box-office-to-budget ratio as a measure of relative theatrical performance, not actual accounting profit.


1. R version: [4.x.x]. Packages: [list them, or point to the setup chunk that installs them].
2. API keys: create a `.Renviron` file in this folder with these names (values are your own):
   - `[KEY_NAME]=`
3. Knit the four files in `markdowns/` in order, 01 to 04.

## The Final Dataset

- File: `data/processed/[name].csv`
- One row is: [ ]
- Rows and columns: [ ]

| Column | Type | Description | Source |
|---|---|---|---|
| [ ] | [ ] | [ ] | [ ] |

## Data Issues and How We Handled Them

[Missing values, inconsistent formats, duplicate records, mismatched keys: what you found and what you did.]


### TMDb Data Quality (Aliya)

Our initial 20-movie pilot identified several data-quality issues:

- **Missing financial information:** Five movies (25%) were missing a usable production budget or reported revenue. These movies were retained in the dataset, with unknown or zero-placeholder financial values converted to `NA`.
- **Financial coverage:** Fifteen movies (75%) had both a positive budget and positive reported revenue. Coverage was 66.7% for randomly selected movies and 100% for intentionally selected movies.
- **Theatrical eligibility:** Some initially selected movies did not have qualifying U.S. theatrical release records. These were replaced using a documented randomized replacement queue.
- **Duplicate prevention:** TMDb identifiers and project `movie_id` values were checked for duplicates. The final pilot contains one row per movie.
- **Ambiguous titles:** TMDb returned multiple records for *Past Lives (2023)*. We manually verified the correct film (TMDb ID 666277) and documented the match in our R code.
- **Release-date consistency:** We retrieved U.S. theatrical release dates separately and checked that they matched each movie's assigned release year.
- **Financial ratios:** The box-office-to-budget ratio was calculated only when both financial values were positive.

TMDb's reported revenue is retained as `reported_revenue` rather than automatically labeled worldwide theatrical gross because the consistency of revenue reporting still requires assessment.

Additional data-quality findings from Wikidata, synopsis extraction, and the final merged dataset will be documented by the team.


## Validation Table

The observations that break an expected rule (impossible values, duplicates, invalid labels or
codes, failed joins). Produced in `markdowns/04_validate.Rmd`.

| Rule | Rows that break it | What we did |
|---|---|---|
| [ ] | [ ] | [ ] |


| Rule | Rows that break it | What we did |
|---|---|---|
| Production budget must be positive | 5 of 20 | Converted unknown or zero-placeholder budgets to `NA`; retained all movies. |
| Reported revenue must be positive | 3 of 20 | Converted unknown or zero-placeholder revenues to `NA`; retained all movies. |
| Both financial values required for ratio | 5 of 20 | Flagged incomplete records and calculated the ratio only for 15 complete movies. |
| U.S. theatrical release date must exist | 0 of 20 | Retrieved and verified U.S. theatrical release dates using TMDb's release-date endpoint. |
| Theatrical release year must match assigned year | 0 of 20 | Validated release years against the master dataset. |
| Duplicate movie IDs | 0 of 20 | Checked uniqueness of `movie_id` and `tmdb_id`. |
| Ambiguous TMDb title match | 1 title during selection | Manually verified *Past Lives (2023)* and used TMDb ID 666277. |


## Limitations

[Assumptions, gaps in the data, and what the dataset cannot answer.]
