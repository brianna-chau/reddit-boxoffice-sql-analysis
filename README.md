## Reddit vs Box Office: SQL and MongoDB Analysis
Multi-database analysis investigating whether Reddit movie discussion frequency predicts international vs domestic box office revenue share, with cross-database validation across Oracle SQL and MongoDB.

This is a fork of a group academic project (CPSC 368, UBC). I led the end-to-end analysis for Research Question 1 — including Oracle SQL pipeline design, cross-database debugging (SQL vs MongoDB), and statistical modeling. Full project reports are included for context.


## Core Finding
Reddit recommendation frequency does not predict international vs domestic revenue share. Across ~448 movies in horror, action, and comedy genres, the overall Pearson r = 0.03. Genre-level correlations were equally negligible: comedy (r = 0.12), action (r = -0.07), horror (r = 0.006). A multiple linear regression with genre interaction terms produced R² = 0.13, indicating the model explains little variance. The hypothesis that horror would show the strongest signal was not supported.


## Research Questions & Results
RQ1 (my analysis): Does Reddit recommendation frequency predict international vs domestic revenue share?
Reddit discussion volume has no meaningful relationship with how revenue splits internationally vs domestically. See findings above.
RQ2: How do IMDb ratings and Reddit discussion jointly relate to box office success?
IMDb rating alone: R² = 0.205. Reddit discussion count alone: R² = 0.220. Combined multiple regression: R² = 0.404. The two metrics appear to capture complementary dimensions of a film's commercial profile.
RQ3: How do Reddit upvotes vary with movie duration across genres?
Upvote patterns are genre-dependent and inconsistent. Action films trend upward with duration until 160-179 minutes, then drop sharply. Comedy peaks at mid and very long durations. Horror shows no discernible trend.

## Technical Stack

Databases: Oracle SQL, MongoDB
Language: Python 3
Libraries: pandas, statsmodels, scipy, sklearn, altair, matplotlib, seaborn, pymongo, oracledb
Methods: Pearson correlation, OLS simple and multiple linear regression with interaction terms, cross-database aggregation pipeline comparison
Scale: ~720,000+ records joined across three heterogeneous datasets


## Dataset Pipeline
Three datasets joined through IMDb as a bridge key:

| Dataset | Size | Role |
|---|---|---|
| IMDb | 300,000+ movies | Bridge key (`imdb_title_id`), genre, ratings |
| Reddit | 420,000+ rows (2022) | Recommendation frequency, upvotes |
| Box Office Mojo | ~4,000 movies (2010-2018) | Domestic and foreign gross revenue |

Reddit rows were linked to IMDb by extracting tt* ID patterns from the processed text column. Box Office Mojo had no shared key with IMDb, so joining required title normalization (lowercase, punctuation stripped) matched on title + year.

## My Contributions (RQ1)
Oracle SQL pipeline: Designed a multi-table JOIN with a LEFT JOIN subquery for recommendation aggregation. Used NULLIF() for zero-division handling on foreign share calculations and NVL() to replace null recommendation counts with 0. Filtered to horror, action, and comedy using LIKE '%Genre%'. Output: 448 rows across 3 genres.

Cross-database debugging: The SQL pipeline initially returned 34 more rows than the MongoDB pipeline on the same query. I diagnosed the root cause: duplicate (title, year) pairs in Box Office Mojo. The SQL join assumed title + year uniquely identified a movie — that assumption was wrong. MongoDB did not have this issue because it keys on imdb_title_id. Fixing the SQL with DISTINCT brought both systems into agreement. See RQ1_sql_vs_mongoDB.ipynb for the full diagnosis.

MongoDB aggregation pipeline: Built a pipeline using $match → $project → $match → $addFields → $sort. Used $size on the embedded reddit_mentions array for recommendation count, $cond for zero-division protection, and $arrayElemAt to extract primary genre.

Statistical modeling: Pearson correlation across overall and per-genre subsets. Multiple linear regression with interaction terms (foreign_share ~ num_recommendations * C(primary_genre)). Validated OLS assumptions via residuals vs fitted plot, residual distribution histogram, and Durbin-Watson statistic. See rq1_mongodb_regression.ipynb.

Visualization: Altair faceted scatterplot comparing domestic vs international revenue share side by side, colour-encoded by genre.
I also contributed to data cleaning and schema design shared across the group.

## Repository Structure

```
/
├── README.md
├── data_clean/
├── data_raw/
├── outputs/                        # RQ1 CSVs, scatter plot, stats summary
├── reports/                        # 3 PDF phase reports
├── data_cleaning.py
├── eda_inspection.py
├── mongoDB_data_loader.ipynb
├── oracle_load_generator.py
├── oracle_schema_load.sql
├── RQ1_sql_vs_mongoDB.ipynb        # Cross-database comparison, 34-record discrepancy diagnosis
├── rq1_mongodb_regression.ipynb    # MongoDB pipeline, OLS regression, Pearson correlation
└── sql_oracle_analysis.py

## Limitations

Data is limited to English-language films from 2010-2018 in three genres
Reddit data reflects 2022 discussions only, not discussion at time of release
Reddit user base may not be representative of general moviegoing audiences
Box Office Mojo coverage is incomplete; not all IMDb titles have revenue data


## Reports
Full methodology, queries, and analysis are documented in /reports:

Data Cleaning and Oracle
Formal Report
MongoDB Report
