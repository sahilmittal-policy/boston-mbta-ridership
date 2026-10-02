# Analysis Plan: Boston's Fare-Free Bus Routes

**Python Programming II · Esha Gupta and Sahil Mittal · September 2026**

---

## 1. Research Question

> **Who rides Boston's fare-free bus routes, and how did ridership and service performance on these routes change over the years, compared with similar routes that kept regular fares?**

### Why this question?

For many city residents, public transit is the main way to reach jobs, school and health care, yet [less than half the world's urban population](https://unstats.un.org/sdgs/report/2020/goal-11/) has convenient access to it. People without good transit face longer and [less safe](https://blogs.worldbank.org/en/voices/road-safety-is-an-issue-of-equity-for-the-poor) commutes, higher costs, and [exclusion from economic and social opportunities](https://www.lse.ac.uk/research/research-for-the-world/politics/urban-transport-ltn-policy). Service that is [frequent and reliable](https://itdp.org/2024/07/24/our-cities-need-more-people-near-frequent-and-rapid-public-transport/) is what lets people depend on transit for daily needs.

### Why Boston?

Boston's rapid transit lines largely bypass Roxbury, Dorchester and Mattapan, which are lower-income neighborhoods with mostly residents of color, leaving them dependent on buses. [Boston's fare-free bus program](https://www.amherst.edu/system/files/2024-07/2%20Cook%20Thesis%20May%202024.pdf) was designed to address this gap. Using federal pandemic relief funds, the City made Route 28 free as a pilot in August 2021, reimbursing the MBTA for each ride. After early results showed ridership gains and high rider satisfaction, the program expanded to Routes 23 and 29 in March 2022. It has since been [extended through December 31, 2026](https://www.boston.gov/news/mayor-michelle-wu-secretary-phil-eng-announce-extension-fare-free-transit-program), with no decision yet on what comes next, which makes this an active question for policymakers.

We compare the fare-free routes (**23, 28, 29**) with **Route 22**, which serves similar neighborhoods but still charges fares.

---

## 2. Data

We draw on the sources in our Data Source Inventory, which link through GTFS route and stop IDs and census tract "GEOID"s. Raw data files are stored in our [dataset repository](https://drive.google.com/drive/folders/1tfGz8ECDQm7eHV_HRsDywQnInZsIgoUd?usp=sharing).

- **[Bus Ridership by Trip, Season, Route/Line, and Stop](https://gis.data.mass.gov/datasets/8daf4a33925a4df59183f860826d29ee)** (MassDOT, Fall 2016–2025): average boardings at each stop, from automatic passenger counters. We use these counts because fare-free riders don't tap a fare card, so fare data can't measure them.
- **[Bus Arrival/Departure Times](https://mbta-massdot.opendata.arcgis.com/)** (MBTA, 2020–2026): when each bus actually reached key stops compared with its schedule. We use it to calculate trip times and bunching.
- **[2015–17 Systemwide Passenger Survey](https://www.ctps.org/dv/mbtasurvey2018/index.html)** (Central Transportation Planning Staff): rider income, race and car access by route, from before the program began.
- **[2025 System-Wide Passenger Survey](https://gis.data.mass.gov/datasets/MassDOT::mbta-2025-system-wide-passenger-survey)** (MassDOT): the same rider measures during the program.
- **[American Community Survey 5-Year Estimates](https://www.census.gov/data/developers/data-sets/acs-5year.html)** (U.S. Census Bureau, 2020–2024): income, poverty, race, car access, rent burden, commute times and unemployment for Boston's census tracts.
- **[TIGER/Line Tract Boundaries](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html)** (U.S. Census Bureau) and **[GTFS Schedule Data](https://www.mbta.com/developers/gtfs)** (MBTA): tract maps, plus the location of every stop and the path of every route.
- **[Fare-Free Mid-Program Report](https://www.boston.gov/departments/transportation/free-route-23-28-and-29-bus-program)** (City of Boston): the City's findings, which we compare with our own.

---


### 3. Exploratory Data Analysis

1. **Who is riding the buses along these routes?** Income, race and car access of riders on each route (2025 Passenger Survey)
2. **Who lives along these routes?** Income, race and poverty in the census tracts near each route (ACS)
3. **Ridership trends:** weekday boardings by route from 2016 to 2025, and where ridership grew most after fares were removed
4. **Busiest stops:** the stops with the most boardings on each route, and which stops gained the most riders
5. **Service changes (before/after):** running times, arrival times compared with schedule, and how often buses arrive bunched together
6. **Car access:** share of households with no car in the tracts along each route
7. **Who has a car:** how car access relates to income and race across Boston tracts
8. **Where riders live relative to the route:** which neighborhoods sit within walking distance (a quarter mile) of each route
9. **Commute time (before/after):** share of residents with long commutes (45+ minutes) near each route, 2015–2019 vs. 2020–2024

---

## 4. Expected Outputs

**Maps**

- Routes 22, 23, 28 and 29 over Boston census tracts, shaded by median household income
- Demographic and zero-car household maps along each route

**Charts**

- Weekday ridership trend, 2016–2025
- Before-and-after ridership by route
- Trip time distributions by route, before and after
- Grouping by route and year
- Rider demographics by route compared with all MBTA buses (2025 survey)
- Riders vs. nearby residents on income, race and car access

**Tables**

- Summary statistics for each route: ridership, trip times and neighborhood demographics
- Difference-in-differences results for ridership and trip times

---

## 5. Final Deliverables

- A **Jupyter notebook** with all cleaning and analysis steps, shared in our GitHub repository
- **Cleaned datasets** for each source
- A **report** summarizing our findings and comparing them with the City of Boston's mid-program report
