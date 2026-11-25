# Data dictionary

<!-- Example values are illustrative unless they come from the run on 2026-11-24. -->

## Part 1 · About the project

**Project:** Hourly subway entries at every Manhattan station complex, used to set realistic crowd levels by station and time of day in our NYC subway sandbox game.

**Source:** MTA Subway Hourly Ridership: Beginning 2025, NY Open Data (dataset 5wq4-mkjj), https://data.ny.gov/resource/5wq4-mkjj.json · docs: https://data.ny.gov/d/5wq4-mkjj

**Refreshed:** Daily by GitHub Actions. Each run asks for Manhattan subway rows from the last 45 days, keeps at most 20,000 rows, and replaces every table. In practice 20,000 rows covers about two days of data.

**Removed on purpose:** Stations outside Manhattan and non-subway rows (filtered at the API, because the game is set in Manhattan). In the clean table, payment methods (OMNY, MetroCard) and fare classes are added together, so one row is one station in one hour.

---

## Part 2 · The tables

### Table 1: `course.main.subway_ridership`

**What it is:** The clean table

**One row is:** one station complex in one hour

**Built from:** raw

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| station_complex_id | text | The MTA's ID for the station complex | a code, not a number to add up | 100 |
| station_complex | text | Station complex name, with the lines that stop there | text | 14 St-Union Sq (4,5,6,L,N,Q,R,W) |
| borough | text | Borough of the station | always Manhattan | Manhattan |
| transit_timestamp | timestamp | Start of the hour the entries were counted in | New York time | 2026-11-20 17:00:00 |
| ride_day | date | The day part of transit_timestamp | date only | 2026-11-20 |
| ride_hour | integer | The hour part of transit_timestamp | 0–23 (17 = 5 PM) | 17 |
| riders | number | People who entered in that hour, all payment methods and fare classes added together | count of entries | 1,850 |

### Table 2: `course.main.ridership_by_station_hour`

**What it is:** A trend

**One row is:** one station complex at one hour of the day, averaged over the days in the data

**Built from:** subway_ridership

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| station_complex | text | Station complex name | text | Grand Central-42 St (4,5,6,7,S) |
| ride_hour | integer | Hour of the day | 0–23 | 8 |
| avg_riders | number | Average entries at that station in that hour | count of entries, 1 decimal | 4,210.5 |

### Table 3: `course.main.station_summary`

**What it is:** Key metrics

**One row is:** one station complex (121 in Manhattan)

**Built from:** subway_ridership

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| station_complex | text | Station complex name | text | Times Sq-42 St/Port Authority Bus Terminal (1,2,3,7,A,C,E,N,Q,R,W,S) |
| total_riders | number | All entries at this station in the data | count of entries | 205,887 |
| avg_per_day | number | total_riders divided by the number of days with data | entries per day, rounded | 102,944 |
| busiest_hour | integer | The hour of the day with the single highest hourly count at this station | 0–23 | 17 |

### Table 4: `course.main.top_stations`

**What it is:** A ranking

**One row is:** one station complex (top 20 by entries only)

**Built from:** station_summary

| Column | Type | What it means | Units or allowed values | Example |
|---|---|---|---|---|
| station_complex | text | Station complex name | text | 34 St-Herald Sq (B,D,F,M,N,Q,R,W) |
| total_riders | number | All entries at this station in the data | count of entries | 114,111 |
| avg_per_day | number | Average entries per day | entries per day | 57,056 |

---

## Part 3 · What this data can't tell you

1. Only about two days. The pipeline keeps at most 20,000 rows, so it can't show weekends vs. weekdays, holidays, or change over weeks.
2. Entries only. The MTA counts people tapping or swiping in. Exits and transfers inside a station aren't counted, so a big transfer hub like 14 St-Union Sq can be more crowded than its number suggests.
3. Manhattan only, and estimates. The other four boroughs aren't included, and the MTA describes these figures as ridership estimates, not exact headcounts.

## Part 4 · Example questions it can answer

1. Which Manhattan subway station is the busiest? (station_summary)
2. What are the 10 busiest stations in Manhattan? (top_stations)
3. What hour of the day is busiest across Manhattan, morning or evening? (ridership_by_station_hour)
4. How crowded is 14 St-Union Sq at 5 PM? (ridership_by_station_hour)