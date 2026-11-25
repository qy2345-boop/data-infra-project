# App evaluation

<!--
Lab 5. Ask your app five questions and judge each answer like an editor would.
Use your findings (docs/findings.md) for the first three: you already know the right answer,
so you can tell whether the app got it right.
Open "How this answer was made" every time: read the SQL and the rows, not just the sentence.
-->

**App URL:** https://minutes-between.streamlit.app/

**Tables the app uses:** `course.main.subway_ridership`, `course.main.ridership_by_station_hour`, `course.main.station_summary`, `course.main.top_stations`

## The five questions

| # | Kind | Question you asked | What the app said | SQL right? (yes / partly / no) | Verdict (good / weak / wrong) | Why |
|---|---|---|---|---|---|---|
| 1 | From finding 1 | Which Manhattan subway station is the busiest? | Times Sq-42 St/Port Authority, with a total of 154,097.0 riders. | yes | weak | Right station, right table (station_summary), and the number matches the row. But the sentence says "a total of 154,097.0 riders" as if it were all-time ridership: it is one day of turnstile entries (total_riders = avg_per_day shows only one day is in the data), and the ".0" looks sloppy. |
| 2 | From finding 2 | What hour of the day is busiest across Manhattan, morning or evening? | Evening. 5 PM is busiest with 294,579 riders, nearly double the 8 AM morning peak (149,605). | yes | good | Sums avg_riders across all stations by ride_hour in ridership_by_station_hour, which is the right way to compare hours. 294,579 / 149,605 ≈ 1.97, so "nearly double" matches the rows. |
| 3 | From finding 3 | How busy is 14 St-Union Sq compared with other Manhattan stations? | 4th busiest with 83,152 riders, behind Times Sq, Grand Central and 34 St-Herald Sq. | yes | good | Correct rank and number from station_summary. It returned all 121 stations to work out the rank, which is more rows than needed but gives the right answer. |
| 4 | A harder one: a comparison | Which is busier at 8 AM, Grand Central or Times Sq? | Times Sq: 10,830 riders on average vs. 9,786 at Grand Central. | yes | good | Used the right table (ridership_by_station_hour), filtered to hour 8 and matched both station names even though the full names include line letters. The two rows match the sentence. |
| 5 | One your data CAN'T answer | How did ridership at Times Sq change compared with last year? | "This data can't answer that. The dataset only covers about two days of recent data and does not contain historical data from last year." | n/a (no query) | good | It refused instead of guessing, and the reason comes from the data dictionary. Small slip: it says "about two days" because the dictionary says so, but the latest run only has one day. |

**Verdict guide**

- **Good:** right number, right table, the sentence matches the rows.
- **Weak:** roughly right, but it used the wrong table, missed a filter, or the sentence overstates what the rows show.
- **Wrong:** wrong number, an error, or it made up an answer to a question the data can't answer.

## One improvement, tested

Pick your weakest answer. Change ONE thing, push, wait for the app to update (about a minute), and ask the same question again.

**Question:** Which Manhattan subway station is the busiest?

**What was wrong:** The number was right, but the sentence overstated it: "a total of 154,097.0 riders" sounds like all-time ridership, when it is one day of turnstile entries. It also printed ".0".

**What I changed** (tick one and describe it):

- [ ] the data dictionary (`docs/data_dictionary.md`): a clearer meaning, units or allowed values
- [x] `extra_instructions` in `app.toml`: told the AI to say how many days the totals cover, to call them "entries", and to round to whole numbers
- [ ] a new or changed table in `project.toml`

**Before:** The busiest Manhattan subway station is Times Sq-42 St/Port Authority Bus Terminal, with a total of 154,097.0 riders.

**After:** ____

## What leaves your control

Every question sends your question, your table descriptions and some result rows to Google's free Gemini tier. In 2–3 sentences: is that acceptable for your data? What would you change before using this app with non-public data?

For this project it is acceptable: the data is public MTA ridership, aggregated to station and hour, with no names, card numbers or individual trips, so the descriptions and rows sent to Gemini reveal nothing private. The real risk is what users type: someone might ask "I leave 123 Main St at 7:40 every morning, which station should I use?", which sends their address and routine to Google. Before using this app with non-public data, I would switch to a paid or self-hosted model that doesn't keep or train on inputs, send only the rows an answer needs, and warn users not to type personal details.