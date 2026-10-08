# GTFS and Tract Linkage

This workflow uses the attached MBTA Fall 2026 GTFS feed (`feed_start_date` 2026-09-16 through `feed_end_date` 2026-12-12) and 2023 Massachusetts TIGER/Line tract boundaries. GTFS stop coordinates are assigned to TIGER tract polygons. GTFS bus route shapes are intersected with those polygons. A 0.25-mile (402.336 m) circular buffer is built around each GTFS bus stop; the stop-buffer/tract table records the area and share of that buffer in each tract. ACS is intentionally not read or merged; the TIGER `GEOID` fields are retained for a later analysis join.

Historical stop-ridership records link to current GTFS stop geography by exact `stop_id` only. Historical route IDs are audited against current GTFS `route_id`; unmatched IDs are retained in the audit rather than fuzzy-matched. In this workspace 6,666 of 7,925 unique historical stop IDs and 148 of 178 historical route IDs appear in the current feed. The exact historical records matched to stops are aggregated by year, route, tract, day type, and fare-free flag in `ridership_stop_tract_year.csv`.

This is a spatial reference to the Fall 2026 network, not a historical GTFS reconstruction. Stops and routes may have moved or changed since earlier ridership years, and unmatched historical IDs may represent retired/renumbered service. The six GTFS bus stops not assigned to a TIGER tract remain in the stop file with a null `GEOID`; no nearest-tract guess is made unless a stop is within 75 m of the nearest polygon. Buffer shares are fractions of each full circular buffer, so shares can sum to less than one where the buffer falls outside mapped land or the Massachusetts tract layer.

## Outputs

- `gtfs_bus_stops.geojson`: mapped current bus-stop points with tract GEOID and assignment method.
- `gtfs_bus_route_shapes.geojson`: current route/shape line geometries.
- `gtfs_stop_tract_assignments.csv`: one row per current bus stop and its tract assignment.
- `gtfs_route_tract_crosswalk.csv`: route/shape coverage by tract.
- `gtfs_stop_quarter_mile_tract_weights.csv`: tract area weights for stop buffers.
- `ridership_stop_tract_year.csv`: historical stop ridership summarized by tract where exact current stop IDs match.
- `gtfs_id_match_audit.csv`: route and stop identifiers matched against current GTFS.
