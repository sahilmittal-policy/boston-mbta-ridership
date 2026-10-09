# Limitations Log

Running list of data limitations and cleaning decisions, to be written up in the Quarto report. Add new items as they come up.

## Study Design

- **Few treated routes.** Only three routes are fare-free (23, 28, 29), so results show patterns rather than proof, and formal statistical tests have limited power.
- **Other changes overlapped with the program.** COVID recovery, new bus lanes and the Bus Network Redesign happened in the same years. Comparison routes help separate these from the fare-free effect but cannot remove them entirely.
- **Route 29 is much smaller** (about 2,100 weekday boardings in 2016 vs. about 12,000 on Routes 23 and 28). Combined totals mostly reflect Routes 23 and 28, and Route 29's percentage changes can look large from small absolute changes.
- **No individual-level data.** Rider and resident comparisons use aggregated survey and census figures, not the same people.

## American Community Survey (ACS)

- **Boston only.** Chelsea, Revere and Winthrop tracts (16xx, 17xx, 18xx) and non-residential tracts (98xx, 99xx) were removed. Neighborhood demographics are not available for bus routes outside Boston.
- **Tract boundaries changed in 2020.** ACS 2015–2019 uses 2010 tracts (167 in Boston); ACS 2020–2024 uses 2020 tracts (191 in Boston). Only 147 tract IDs appear in both, so comparisons are made across route areas, not individual tracts.
- **Missing and capped income.** In 2020–2024, median household income was missing (Census code `-666666666`) for 3 Boston tracts; two residential tracts were kept with income set to missing. Two tracts are top-coded at $250,001 ("$250,000 or more").
- **Zero-population tract removed.** One 2020–2024 tract (061202) had no residents and was dropped.
- **Tract averages are unweighted.** Summary statistics average tracts equally regardless of population, so they differ slightly from citywide figures.
- **Estimates carry sampling error.** ACS 5-year values are survey estimates pooled over five years, each with a margin of error.
- **Work from home changed commuting.** Transit commuting fell from about 33% (2015–2019) to about 23% (2020–2024) of workers, largely because of remote work, which also lowered bus ridership system-wide.

## Bus Ridership

- **Fall only.** Ridership data covers one season (Fall) per year, so it shows year-to-year trends but not month-to-month changes.
- **Passenger counter averages.** Values are averages from automatic passenger counters across sampled trips, not complete counts.
- **File formats changed over time.** 2024 moved the year into a separate column and renamed `sample_size` to `sample_count`; 2025 used entirely different column names and short day-type labels (`wkdy`, `sat`, `sun`). All were standardized during cleaning.
- **2025 weekday is a weighted average.** The 2025 data reports Monday–Thursday and Friday separately. They were combined as (4 × Mon–Thu + 1 × Friday) / 5 to match the five-day weekday average of earlier years.
- **2025 dip may partly reflect the format change.** Route 28 weekday boardings were about 9% lower in 2025 than in 2024; part of this may come from the new data format rather than a real change.
- **2020 is a COVID year.** Fall 2020 ridership was far below normal and is excluded from before/after comparisons.
- **Fewer stops over time.** The number of stop records declines from 2018 onward, reflecting stop consolidation and route changes.
- **Stop names may vary across years**, so stop-level comparisons should match on `stop_id`.
- **Silver Line kept.** Silver Line routes (741, 742, 743, 746, 749, 751) are bus rapid transit but remain in the comparison group.

## Bus Arrival/Departure (Service Performance)

- **Data begins in 2020.** The "before" period for service measures falls during COVID, when traffic and ridership were unusually low.
- **2020 files differ.** 2020 comes in four quarterly files; October–December uses different column names. About 20% of October–December 2020 trips could not be matched from start to end point, so trip counts for that quarter understate actual service. Median trip times, lateness and bunching remain consistent with earlier months.
- **Time zones differ by year.** 2020 times are in Boston local time; 2021–2025 times are in UTC and were converted.
- **Daylight saving approximated by month.** The peak-hour label treats April–October as daylight time. The actual switches happen in mid-March and early November, so the peak label is off by an hour for a few weeks each year. Lateness, trip time and bunching are not affected.
- **Missing trip IDs.** About 8% of 2025 records have no `half_trip_id` and are excluded from trip-time calculations (but included in lateness and bunching).
- **Impossible trip times removed.** Trip times below 0 or above 300 minutes (recording errors) were dropped.
- **Bunching calculated from our own gaps.** The MBTA's `headway` and `scheduled_headway` fields were mostly blank (and absent for frequent "headway" routes such as Route 28), so gaps between buses were computed from scheduled and actual times. A bus counts as bunched if its actual gap is under half its scheduled gap; buses that passed each other also count as bunched.
- **Peak definition.** Peak = weekdays, 7–9 AM and 4–7 PM Boston time.
- **No dwell time.** Data is recorded only at timepoints, so time spent boarding at individual stops cannot be measured directly; boarding speed is tested indirectly through trip time.
- **Early buses in fall 2020.** Buses ran several minutes early from September 2020 onward, likely because schedules included extra time while traffic was light. Treated as a real 2020 pattern, not a data error.

## Passenger Surveys

- **Route grouping differs between surveys.** 2015–17 reports Routes 22 and 29 together ("22 and 29"); 2025 reports Routes 28 and 29 together ("28 & 29"). Route 23 is the only fare-free route reported separately in both.
- **2025 release pools 2022–2025 responses**, so it describes riders across the program period rather than a single year.
- **Income measured differently.** 2015–17 uses dollar brackets; 2025 uses shares of Area Median Income. Low-income and minority classifications are the most comparable measures.
- **Sampling error.** Both surveys are weighted samples; 2015–17 reports response counts but no margins of error.
- **Some 2025 income labels overlap as published** (for example, "Less than 40%" and "30% to 60%" of Area Median Income).
