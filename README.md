# Boston's Fare-Free Bus Routes

**Python Programming II · Final Project · Esha Gupta and Sahil Mittal · Fall 2026**

In 2021–2022 the City of Boston made three MBTA bus routes free: **Routes 23, 28 and 29**, which run through Roxbury, Dorchester and Mattapan. This project asks whether removing the fare changed how many people ride those buses, and who those riders are, by comparing them with **Route 22**, a nearby route that serves the same neighbourhoods but kept its fare.

## Key findings

| | Riders, 2019 → 2025 | Service (trips), 2020 → 2025 | Better than Route 22 after adjusting for service |
|---|---|---|---|
| **Route 23** (free) | +1.0% | −7.2% | **+20.4 points** |
| **Route 28** (free) | −7.4% | −7.2% | **+11.4 points** |
| **Route 29** (free) | −40.9% | −35.8% | **+3.6 points** |
| **Route 22** (paid) | −9.0% | +2.9% | — |

1. **The fare-free routes held on to more of their riders.** From 2019 to 2025, weekday ridership fell 6.3% on the fare-free routes combined, compared with 18.7% on the 8 busiest other routes and 22.8% on all other routes.
2. **It wasn't because they got more buses.** Route 22 *gained* service but still lost riders; Route 23 *lost* service but gained riders.
3. **The result holds against every comparison group.** After adjusting for service, the fare-free routes did 15–17 percentage points better than Route 22, the top 8 routes and the whole network.
4. **Route 29 shows the limit of free fares.** Its ridership fell almost exactly as much as its service was cut. Free fares cannot make up for fewer buses.

The full write-up is in [`docs/Final/report.qmd`](docs/Final/report.qmd) (rendered: [`docs/Final/report.html`](docs/Final/report.html)).

---

## Contents

1. [Research question](#research-question)
2. [Repository structure](#repository-structure)
3. [Data sources](#data-sources)
4. [How to reproduce the analysis](#how-to-reproduce-the-analysis)
5. [How we got here: the process](#how-we-got-here-the-process)
6. [Outputs](#outputs)
7. [Limitations](#limitations)
8. [Use of AI tools](#use-of-ai-tools)

---

## Research question

> **Who rides Boston's fare-free bus routes, and how did ridership and service on these routes change over the years, compared with similar routes that kept regular fares?**

**Why it matters.** For many city residents, buses are the main way to reach jobs, school and health care. Boston's rapid transit lines largely bypass Roxbury, Dorchester and Mattapan, so residents there depend on buses. The fare-free program was designed to close that gap. It began as a Route 28 pilot in **August 2021**, expanded to Routes 23 and 29 in **March 2022**, and has been extended through **December 31, 2026**, with no decision yet on what comes next.

**Why Route 22 as the comparison?** It runs through the same neighbourhoods as the fare-free routes, has similar riders (in the 2025 survey: 80% low-income, 88% minority, 56% without a car, almost identical to the fare-free routes), and kept its fare. That makes it the closest available picture of what would have happened to the fare-free routes without the program.

---

## Repository structure

```
boston-mbta-ridership/
├── data/
│   ├── raw/                    
│   └── processed/              
│
├── notebooks/
│   ├── clean/                
│   │   ├── acs_clean_2024.ipynb
│   │   ├── acs_clean_2019.ipynb
│   │   ├── acs_combined.ipynb
│   │   ├── bus_ridership.ipynb
│   │   ├── arrival_departure.ipynb
│   │   ├── passenger_survey_2017_2019.ipynb
│   │   ├── passenger_survey_2025.ipynb
│   │   ├── survey_clean.ipynb
│   │   ├── gtfs_clean.ipynb
│   │   ├── tract_clean.ipynb
│   │   └── map_radius.ipynb
│   └── final/ 
│       ├── demographics.ipynb
│       └── ridership_analysis.ipynb 
│
├── figures/              
│
├── docs/
│   ├── Submissions/           
│   │   ├── Data Inventory and Data Dictionary.pdf
│   │   ├── Data Analysis Plan.md
│   │   └── Data Analysis Plan.pdf
│   └── Final/                  
│       ├── report.qmd          
│       ├── report.html        
│       ├── references.bib      
│       └── limitations.md     
│
├── README.md
├── requirements.txt
├── LICENSE
└── .gitignore
```

---

## Data sources

All data is public. Raw files are too large for GitHub, so they are shared in the project's [Google Drive folder](https://drive.google.com/drive/folders/1tfGz8ECDQm7eHV_HRsDywQnInZsIgoUd?usp=sharing), or can be downloaded from the original sources below.

| Dataset | Publisher | What it contains | Used for |
|---|---|---|---|
| [Bus Ridership by Trip, Season, Route/Line, and Stop](https://gis.data.mass.gov/datasets/8daf4a33925a4df59183f860826d29ee) | MassDOT | Average boardings per stop and trip, Fall 2016–2025, from automatic passenger counters | Ridership trends |
| [Bus Arrival/Departure Times](https://mbta-massdot.opendata.arcgis.com/) | MBTA | Scheduled and actual time of every bus at key stops, 2020–2025 | Service (trips run) |
| [2015–17 Systemwide Passenger Survey](https://www.ctps.org/dv/mbtasurvey2018/index.html) | CTPS | Rider income, race, car access, trip purpose by route | Riders before the program |
| [2025 System-Wide Passenger Survey](https://gis.data.mass.gov/datasets/MassDOT::mbta-2025-system-wide-passenger-survey) | MassDOT | The same measures, responses from 2022–2025 | Riders during the program |
| [American Community Survey 5-Year](https://www.census.gov/data/developers/data-sets/acs-5year.html) | U.S. Census Bureau | Income, poverty, race, car access, rent, commuting, employment by census tract (2015–19 and 2020–24) | Who lives along each route |
| [TIGER/Line tract boundaries](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html) | U.S. Census Bureau | Census tract maps (2019 and 2024) | Maps, walking areas |
| [MBTA GTFS](https://www.mbta.com/developers/gtfs) | MBTA | Stop locations and route paths | Maps, walking areas |



## How to reproduce the analysis

### 1. Install the software

- **Python 3.10+** (we used Anaconda)
- Python packages:

```bash
pip install -r requirements.txt
```

or, with conda (recommended for the map packages on Windows):

```bash
conda install -c conda-forge pandas numpy matplotlib seaborn geopandas pyogrio jupyter requests
```

- **Quarto** (only to render the report): https://quarto.org/docs/get-started/

### 2. Set your file paths

The notebooks use file paths from our own computers, for example:

```python
processed = r'C:\Users\sahil\projects\boston-mbta-ridership\data\processed'
```

Before running, **change these paths at the top of each notebook** to where the repository is on your machine. Every notebook reads from `data/raw` or `data/processed` and writes to `data/processed` or `figures`.

### 3. Run the notebooks in this order

Each cleaning notebook reads raw data and saves a cleaned file. Later notebooks build on earlier ones, so the order matters.

| Step | Notebook | Reads | Creates |
|---|---|---|---|
| 1 | `clean/acs_clean_2024` | ACS 2020–24 (raw) | `acs_2024_tracts_clean.csv` |
| 2 | `clean/acs_clean_2019` | ACS 2015–19 (raw) | `acs_2019_tracts_clean.csv` |
| 3 | `clean/acs_combined` | both ACS clean files | `acs_2019_2024_combined.csv` |
| 4 | `clean/bus_ridership` | 10 ridership files | `ridership_route_year.csv`, `ridership_stops_year.csv` |
| 5 | `clean/arrival_departure` | 64 arrival/departure files | `service_route_month.csv` |
| 6 | `clean/passenger_survey_2017_2019` | 2015–17 survey | `passenger_survey_bus_line_values_clean.csv`, `..._percentages_clean.csv` |
| 7 | `clean/passenger_survey_2025` | 2025 survey | `passenger_survey_2025_bus_clean.csv` |
| 8 | `clean/survey_clean` | both survey files | `surveys_clean.csv` |
| 9 | `clean/gtfs_clean` | GTFS feed | `route_stops.csv`, `route_shapes.csv` |
| 10 | `clean/tract_clean` | TIGER + ACS clean files | `boston_tracts_2024.geojson`, `boston_tracts_2019.geojson` |
| 11 | `clean/map_radius` | stops + tracts + ridership | `route_area_demographics.csv` |
| 12 | `exploratory/demographics` | route areas + surveys | demographic charts |
| 13 | `final/ridership_analysis` | ridership + service | report figures in `figures/` |


### 4. Render the report

```bash
cd docs/Final
quarto render report.qmd --to html
```



---

## How we got here: the process

### Step 1: Scoping

We started with all MBTA bus data and narrowed to one policy question: did the fare-free program work? We chose a **before-and-after comparison with a comparison route**: the change on the fare-free routes minus the change on a similar route that kept its fare. This removes changes that affected every route (COVID, remote work) and leaves the difference linked to free fares. In economics this is called a *difference-in-differences* design (Angrist and Pischke, 2009).

### Step 2: Cleaning each dataset

Each dataset needed its own fixes.

### Step 3: Exploring who lives along the routes and who rides them

`demographics.ipynb` compares the four routes' neighbourhoods (income, poverty, car ownership, rent burden, minority share) and riders (2025 survey). The fare-free routes and Route 22 serve very similar, lower-income, transit-dependent neighbourhoods, which confirms Route 22 as a fair comparison.

### Step 4: The main analysis

`ridership_analysis.ipynb` builds the story in two parts.

**Focus routes (23, 28, 29 vs. 22)**
1. **Weekday riders, 2016–2025:** all four routes collapsed in 2020; Route 28 rebounded sharply in its first fare-free fall, and Route 23 returned to its pre-COVID level.
2. **Ridership vs. service:** comparing the change in riders with the change in trips run shows Route 22 gained service yet lost riders.
3. **Service-adjusted comparison:** because service changed differently on each route, we also compare ridership *relative to service* (riders per trip)

**The wider network**
1. **Share of all bus riders** carried by the fare-free routes, by year.
2. **Robustness check:** the same service-adjusted comparison against Route 22, the 8 busiest other routes and all other routes. The gaps are consistent (15–17 points), so the result does not depend on choosing Route 22.
3. **Riders beyond each route's share of service:** each route's share of riders minus its share of trips.

**Periods used:** riders are compared from Fall 2019 to Fall 2025. Service data only begins in 2020, so service is compared from January–February 2020 (before COVID and the program) to January–February 2025.

---

## Limitations

The full list is in [`docs/Final/limitations.md`](docs/Final/limitations.md). The most important:

- **Only three fare-free routes**, so results show consistent patterns rather than statistical proof.
- **Other changes happened at the same time:** COVID recovery, new bus lanes (including the Columbus Avenue lane used by Route 29) and the Bus Network Redesign. The comparison routes help separate these from the fare effect but cannot remove them entirely.
- **The service adjustment assumes riders change one-for-one with service.**
- **Service data starts in 2020**, so the service "before" period is January–February 2020 rather than Fall 2019.
- **Route 29 lost about a third of its service after 2022**, so its ridership mainly reflects service cuts.
- **The surveys group routes differently** in each year, and the 2025 survey pools responses from 2022–2025.
- **Census data covers Boston only**, so neighbourhood figures describe only the Boston part of routes that leave the city.

---

## Use of AI tools

We used Claude, an AI assistant developed by Anthropic, for support with Python code debugging, chart design and editing. All analysis decisions, interpretation and final writing are our own, and every result was checked against the data. Claude is cited in the report's references.

---

## Team

- **Sahil Mittal**
- **Esha Gupta**


## License

[MIT](LICENSE)
