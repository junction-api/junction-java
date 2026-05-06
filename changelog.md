## 1.0.0 - 2026-05-06
### Breaking Changes
* **`MealInDbBaseClientFacingSource` builder** — a new required `calendarDate` step has been inserted between `timestamp` and `name` in the staged builder; `timestamp()` now returns `CalendarDateStage` instead of `NameStage`. Update all builder chains to call `.calendarDate("YYYY-MM-DD")` after `.timestamp(...)`.
### Added
* **`LabReportResultSampleType`** — new enum type representing the biological sample type (e.g. `URINE`, `SERUM_PLASMA_BLOOD`, `CAPILLARY_BLOOD`, `STOOL`, `SALIVA`) on a lab result.
* **`LabReportResultMeasurementKind`** — new enum type representing how a lab result was measured (`DIRECT`, `CALCULATED`, `RATIO`, `UNKNOWN`).
* **`LabReportResult.getSampleType()`** and **`LabReportResult.getMeasurementKind()`** — new optional fields and corresponding builder methods on `LabReportResult`.
### Changed
* **`Environment`** base URLs updated — all environment constants (`PRODUCTION`, `PRODUCTION_EU`, `SANDBOX`, `SANDBOX_EU`) now point to `junction.com` domains instead of `tryvital.io`.

## 0.0.1 - 2026-05-01
* Initial SDK generation
* 🌿 Generated with Fern

