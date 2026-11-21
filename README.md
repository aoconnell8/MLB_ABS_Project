# The Impact of the Automated Ball-Strike Challenge System on Major League Baseball

ISA 401 Final Project, Fall 2026, Team 1, Section B


## Submission

| Item | Link |
|---|---|
| Dashboard (Tableau Public or Power BI) | [link] |
| Recorded technical presentation (YouTube, unlisted) | [link] |
| This repository | [link] |

## Team

| Member | What I built | Portfolio project entry |
|---|---|---|
| Joseph Sher | Retreived MLB player statistics through an API, scraped Baseball-Reference batting data, collected player height information, and cleaned and merged multiple datasets for the offensive player performance portion of the project. Developed analyses examining challenge activity, challenge success, offensive performance, and player characteristics. |
| [First Last] | [ ] | [ ] |
| [First Last] | [ ] | [ ] |

Each member adds this project to their own portfolio site, in their own words, with a link
back to this repository and the dashboard, and updates the Skills section of their portfolio
and of their `business_intelligence` README with skills they used in this project.

## The Question

To what extent has the Automated Ball-Strike Challenge System changed the accuracy of umpire calls, offensive player performance, and game outcomes in Major League Baseball (MLB)?

This project examines how MLB's Automated Ball-Strike (ABS) Challenge System may influence the game by allowing players to challenge ball and strike calls made by home plate umpires. By combining ABS challenge information, MLB player statistics, and additional baseball data collected from multiple sources, we evaluate changes in umpire call accuracy, offensive performance, and game outcomes.

The project focuses on three main areas: the effectiveness of challenges in overturning umpire decisions, the relationship between challenge activity and offensive player performance, and the broader impact of ABS on MLB games. This topic is particularly relevant because it examines how the introduction of automated technology may affect individual player decisions, offensive opportunities, and the overall game of baseball.


## Data Sources

The project combines data from multiple real-world baseball sources collected through web scraping, APIs, and externally provided datasets.

| Source | Link | Method | One row is | Code |
|---|---|---|---|---|
| Baseball-Reference MLB Standard Batting Statistics | https://www.baseball-reference.com/leagues/majors/2024-standard-batting.shtml (similar season-specific pages for 2025 and 2026) | Web Scraping (rvest) | One player's batting statistics for a team during a particular season | `markdowns/01_acquire.Rmd` |
| MLB Stats API - Hitting Statistics | https://statsapi.mlb.com/api/v1/stats | API Retrieval (httr2 and jsonlite | One player's regular-season hitting statistics for a particular season | `markdowns/01_acquire.Rmd` |
| MLB Stats API - Player Information  | https://statsapi.mlb.com/api/v1/people | API Retrieval (httr2 and jsonlite) | One MLB player's biographical record, including height information | `markdowns/01_acquire.Rmd` |
| ABS Challenge Statistics | https://baseballsavant.mlb.com/leaderboard/abs-challenges?sort=runs_above_average&sortDir=desc&page=0&pageSize=50&search= | CSV data import using readr | One player's aggregated ABS challenge statistics for 2026 season | `markdowns/01_acquire.Rmd` |

The batting statistics cover the 2024, 2025, and 2026 MLB regular seasons. ABS challenge statistics are used to examine player challenge behavior and outcomes during the 2026 season. Player height information is used to explore whether physical characteristics are associated with differences in challenge success.

## Pipeline

This project uses 4 R markdown files to cohesively organize data acquisition, cleaning, merging, and validation.

| Step | File | What it does |
|---|---|---|
| 1. Acquire | `markdowns/01_acquire.Rmd` | Collects raw data through web scraping, retrieves information through an API, and imports data from a CSV file. It also saves the original datasets in data/raw/. |
| 2. Clean and transform | `markdowns/02_clean.Rmd` | Cleans and standardizes the individual datasets, renames variables, converts data types, standardizes player names and matching keys, and prepares the datasets for merging. Saves cleaned datasets in data/processed/. |
| 3. Merge | `markdowns/03_merge.Rmd` | Combines batting statistics from the MLB API and Baseball-Reference using player identifiers, player names, and season. Adds ABS challenge statistics and player height information, handles duplicate team records, compares overlapping statistics, and creates additional variables such as height hroups and ABS data indicators. |
| 4. Validate and summarize | `markdowns/04_validate.Rmd` | Checks for duplicate player-season records, missing values, invalid statistics, unmatches records, and challenge total inconsistencies. Produces summary statistics and exports final validated dataset for Tableau analysis.|

Raw files go in `data/raw/`; the cleaned and merged files go in `data/processed/`.

## How to Reproduce

1. R Environment: Run the project using RStudio and installed version of R 4.6. The main packages used include:
- `dplyr` for data cleaning, transformation, and merging
- `readr` for importing and exporting CSV files
- `rvest` for web scraping 
- `httr2` for making MLB Stats API requests
- `jsonlite` for processing API responses
- `purrr` for applying data collection across multiple seasons

2. API Access: The MLB Stats API endpoints used in this project are publicly accessible and do not require a personal API key. No API keys should be committed to the repository

3. Data Files: Ensure the required raw data files are available in data/raw/.

4. Run the Pipeline: Open the project in RStudio and knit the 4 R Markdown files in order:
- `01_acquire.Rmd`
- `02_clean.Rmd`
- `03_merge.Rmd`
- `04_validate.Rmd`

5. The final validated dataset is exported to `data/processed/final_dataset.csv` and can be connected to Tableau for visualization and analysis

## The Final Dataset

- File: `data/processed/final_dataset.csv`
- One row is: One MLB player-season observation, representing a player's batting statistics for a particular season, along with available ABS challenge statistics and player height information
- Rows and columns: 2,256 rows and 30 columns
- Coverage: 2024, 2025, and 2026 MLB regular seasons
- Unique Players: 1,144 unique MLB player IDs across 3 seasons

The final dataset combines traditional offensive statistics, ABS challenge outcomes, and player height information. This allows us to examine offensive performance across multiple seasons while exploring player-level differences in challenge behavior during the 2026 ABS season.

| Column | Type | Description | Source |
|---|---|---|---|
| player_id | Character | Unique MLB player identifier | MLB Stats API |
| player_name | Character | Player's full name | MLB Stats API/Baseball-Reference |
| player_key | Character | Standardized player name used to match records across sources | Created during cleaning |
| year | Integer | MLB season associated with the record | All seasonal sources |
| team | Character | Player's team during the season | Baseball-Reference |
| plate_appearances | Integer | Total plate appearances during the season | MLB Stats API |
| games | Numeric | Total games played during the season | Baseball-Reference |
| at_bats | Numeric | Total official at-bats during the season | Baseball-Reference |
| hits | Numeric | Total hits during the season | Baseball-Reference |
| avg | Numeric | Batting Average | MLB Stats API/Baseball-Reference |
| obp | Numeric | On-base percentage | MLB Stats API/Baseball-Reference
| slg | Numeric | Slugging percentage | MLB Stats API/Baseball-Reference |
| ops | Numeric | On-base plus slugging percentage | MLB Stats API/Baseball-Reference |
| home_runs | Integer | Total home runs during the season | MLB Stats API |
| walks | Integer | Total walks during the season | MLB Stats API |
| strikeouts | Integer | Total strikeouts during the season | MLB Stats API
| runs_above_avg | Numeric | Runs above average as reported in the ABS dataset |ABS Challenge Dataset |
| runs_gained | Numeric | Runs gained as reported in the ABS dataset | ABS Challenge Dataset |
| runs_cost | Numeric | Runs cost as reported in the ABS challenge dataset | ABS Challenge Dataset |
| challenges | Numeric | Total number of ABS challenges | ABS Challenge Dataset |
| challenge_wins | Numeric | Number of successful challenges resulting in overturned calls | ABS Challenge Dataset |
| challenge_losses | Numeric | Number of unsuccessful challenges | ABS Challenge Dataset |
| win_percentage | Numeric | Percentage of challenges successfully overturned | ABS Challenge Dataset
| strikeouts_removed | Numeric | Strikeouts removed through successful challenges | ABS Challenge Dataset |
| walks_gained | Numeric | Walks gained through successful challenges | ABS challenge dataset
| abs_data_matched | Logical | Indicates whether ABS challenge information was matches to the player-season record | Created during merging
| period | Character | Identifies the season as pre-ABS (2024-2025) or ABS-era (2026)
| height_text | Character | Player height in its original feet-and-inches format | MLB People API
| height_inches | Integer | Player height converted into total inches | MLB People API/Created during cleaning |
| height_group | Character | Categorizes players into height groups for comparison | Created during cleaning |



## Data Issues and How We Handled Them

Several data inconsistencies had to be addressed when combining information from multiple baseball sources.

- Inconsistent player names: Player names were standardized by converting them to lowercase and removing leading and trailing spaces. A standardized player_key was created to help match records across sources.

- Different player ID formats: Player identifiers were standardized during the merging process to prevent incompatible data type errors when joining datasets.

- Duplicate team records: Some players appeared multiple times in Baseball-Reference because they played for multiple teams during a season. Combined-team records (2TM) were retained instead of separate team records to prevent duplicate player-season observations.

- Overlapping batting statistics: Baseball-Reference and the MLB Stats API contained overlapping offensive statistics. These values were compared during merging and were generally consistent, with only a small difference in OPS due to rounding.

- Missing ABS records: ABS challenge information was only available for the 2026 season. The 2024 and 2025 observations therefore have missing ABS fields because those seasons are classified as pre-ABS. Additionally, 225 of the 750 observations from 2026 did not have a matched ABS record. These missing values were preserved rather than automatically replaced with zero because an unmatched record does not necessarily establish that a player had zero challenges.

- Missing Baseball-Reference fields: A total of 59 observations had missing team, games, at-bats, and hits information. These records were retained in the final dataset rather than removed, preserving the available player information from the other sources.

- Player height formatting: Height information was originally provided in feet-and-inches format. It was converted into a numeric inches variable to support comparisons and the creation of height groups.

- Different levels of data aggregation: The batting sources contained player-season or player-team-season observations, while the ABS dataset contained aggregated player-level challenge information. The data was standardized to the player-season level before the final merge.

- Zero plate appearances: The dataset contains 272 observations with zero plate appearances. These records were retained, but they must be handled carefully in analyses involving per-plate-appearance rates because the denominator is zero.

These cleaning and transformation steps helped create a consistent dataset that combines offensive statistics, player characteristics, and ABS challenge information.

## Validation Table

The final dataset was evaluated using several validation checks in `markdowns/04_validate.Rmd`. These checks examined duplicate records, missing identifiers, invalid values, missing information, and inconsistencies in challenge statistics.

Rule | Rows that break it | What we did

Duplicate player-season records | 0 | Confirmed that each player has only one observation per season using player_id and year.
Missing player identifiers | 0 | Confirmed that every observation contains a player ID.
Missing player names | 0 | Confirmed that every observation contains a player name.
Missing player matching keys | 0 | Confirmed that every observation contains a standardized player_key.
Missing player height information | 0 | Confirmed that all 2,256 observations contain height information.
Negative batting statistics | 0 | Checked batting totals for impossible negative values.
Challenge wins plus losses must equal total challenges | 0 among 525 matched records | Confirmed that successful and unsuccessful challenges add up to total challenges for every matched ABS observation.
Challenge win percentage consistency | 0 among 525 matched records | Confirmed that the reported win percentage agrees with successful challenges divided by total challenges.
Missing team, games, at-bats, and hits | 59 | Retained the 59 observations with missing values in these four fields rather than removing otherwise usable player information.
Missing ABS challenge information | 1,731 | Preserved missing values for all 1,506 pre-ABS observations and 225 unmatched 2026 observations rather than treating them as zero challenges.
Zero plate appearances | 272 | Retained these observations but identified them as records requiring special consideration in rate-based analyses.

The validation process confirmed that the final dataset contains no duplicate player-season observations, missing player identifiers, or missing player height values. Challenge totals and win percentages were also consistent across all 525 observations with matched ABS information.

## Limitations

Although this project combines multiple data sources and provides several perspectives on the ABS Challenge System, there are limitations to the analysis.

1) Limited time period: The analysis focuses on the 2024, 2025, and 2026 MLB regular seasons. This provides two pre-ABS seasons and one ABS-era season, but additional seasons would be needed to evaluate longer-term trends.

2) Causation versus correlation: Changes in offensive performance or game outcomes cannot automatically be attributed to ABS. Other factors, including player development, pitching performance, injuries, team strategies, and other rule changes, may influence the results.

3) Different data coverage: ABS challenge information is only available for the 2026 season in the dataset, while traditional batting statistics cover all three seasons. Therefore, some analyses use different player samples.

4) Unmatched ABS records: Of the 750 player-season observations in 2026, 525 have matched ABS challenge information. The remaining 225 observations have missing challenge statistics and cannot automatically be interpreted as players who made zero challenges.

5) Player selection and sample size: The final dataset contains 2,256 player-season observations representing 1,133 unique players. However, not every player has the same amount of playing time or available information, which may affect comparisons.

6) Zero plate appearances: Some observations have zero plate appearances. These records cannot be used directly in calculations involving rates per plate appearance and must be handled appropriately.

7) Missing batting information: Fifty-nine observations have missing team, games, at-bats, and hits information. Although these records were retained, their missing fields may limit certain analyses.

8) Player-level aggregation: The ABS challenge dataset contains aggregated player statistics rather than individual pitch-by-pitch challenge events. This limits the ability to examine specific pitch locations, pitch types, counts, and individual game situations.

9) Offensive performance factors: Statistics such as OPS, batting average, walks, and strikeouts are influenced by many factors beyond challenge outcomes. Observed relationships do not establish that ABS directly caused changes in player performance.

10) Game-level outcomes: Player-level statistics alone cannot fully explain changes in game outcomes. Additional game-level information is necessary to evaluate scoring, wins and losses, and other team-level results.

Despite these limitations, the project provides a data-driven examination of how the ABS Challenge System relates to umpire accuracy, offensive player performance, and the broader MLB experience.
