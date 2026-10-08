# Passenger Survey Harmonization Note

The old survey is the **2015-17 MBTA Systemwide Passenger Survey** documented by CTPS. Its local processed filename (`ps_2017_2019_values.csv`) is inconsistent with the source period. The 2025 survey is the MassDOT MBTA System-Wide Passenger Survey. This notebook uses the old values export only; it does not use the separate old percentage export.

Route groups are matched by complete route-member sets. Old individual-route records may be summed only to reconstruct a 2025 grouped reporting group when every member has exactly one old record. No grouped source record is split. Route 23 is an individual match. Old Routes 22 and 29 are grouped together, while 2025 reports Route 22 separately and Routes 28 and 29 together; those records are not equivalent and are excluded from the combined table. One-sided route groups and neighborhood reporting groups remain in the route crosswalk as not comparable. The combined dataset contains only route groups validly available in both survey periods.

`directly_comparable` categories retain equivalent definitions. `comparable_after_aggregation` is used only for exact sums: the older 0.50-0.99 vehicles-per-capita category against the exact combined 2025 0.5 and strictly-between-0.5-and-1 categories; older vehicle-per-capita 1.00+ categories against 2025 1 or more; old carpool plus personal-vehicle drop-off against 2025's combined category; and the old 1-day-or-7-day pass category against 2025's separate 1-day and 7-day categories. Age bands that cross boundaries, income bands (dollar bands versus AMI), low-income status (different thresholds), race/ethnicity definitions not explicitly equivalent, and categories such as bicycle versus micromobility are not forced to match. Employment/student status is not present in either export.

Original category labels and values are preserved in `category_original`, `source_categories_original`, `value`, and `source_values_original`, and `source_route_values_original` for route-level components when records are summed. Old values are weighted estimates, not respondent counts; old `count_*` fields are retained as `n`. The 2015-17 percentage is derived only for complete single-choice value distributions. The old values export alone does not provide a defensible respondent denominator for check-all-that-apply measures, so their percentage is missing. 2025 `weighted_percent` is kept as `percent`. Values in `value` therefore retain source units and must not be compared directly across years. No older margin of error is estimated, and no MOE is assigned to sums of categories.

Availability is assessed per matched route group and characteristic. Percentage sums are tested only for single-choice distributions; incomplete distributions are reported as not tested, and multi-select characteristics are not expected to sum to one. Route flags are group-level: a grouped record receives a flag if it contains a focal/fare-free route, without implying a route-specific estimate within that group.


## Sources

- 2015-17 CTPS survey: https://www.ctps.org/dv/mbtasurvey2018/index.html
- 2025 MassDOT survey: https://gis.data.mass.gov/datasets/MassDOT::mbta-2025-system-wide-passenger-survey
