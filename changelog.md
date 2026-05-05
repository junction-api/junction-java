## 1.0.0 - 2026-05-05
### Breaking Changes
* **`MealInDbBaseClientFacingSource.Builder`** — `timestamp()` now returns `CalendarDateStage` instead of `NameStage`, inserting a new required `calendarDate(String)` step in the builder chain. Migrate by adding `.calendarDate("YYYY-MM-DD")` between `.timestamp(...)` and `.name(...)`.
### Added
* **`LabReportResult`** — new optional `getSampleType()` (`LabReportResultSampleType`) and `getMeasurementKind()` (`LabReportResultMeasurementKind`) fields, with corresponding builder methods.
* **`LabReportResultSampleType`** — new non-exhaustive enum type representing the biological sample type (e.g. `URINE`, `SERUM_PLASMA_BLOOD`, `STOOL`, `SALIVA`).
* **`LabReportResultMeasurementKind`** — new non-exhaustive enum type representing how a lab value was derived (e.g. `DIRECT`, `CALCULATED`, `RATIO`).
### Changed
* **`Environment`** — all base URLs updated from `tryvital.io` to `junction.com` domains (`PRODUCTION`, `PRODUCTION_EU`, `SANDBOX`, `SANDBOX_EU`).

## 0.0.1 - 2026-05-01
* Initial SDK generation
* 🌿 Generated with Fern

