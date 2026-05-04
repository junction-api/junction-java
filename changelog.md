## 1.0.0 - 2026-05-04
### Breaking Changes
* **`MealInDbBaseClientFacingSource` builder** — a new required `calendarDate` step has been inserted into the staged builder chain; `TimestampStage.timestamp()` now returns `CalendarDateStage` instead of `NameStage`, so existing builder call chains will fail to compile. Add a `.calendarDate(...)` call after `.timestamp(...)` to migrate.
* **`Environment` URLs** — the base URLs for all four environment constants (`PRODUCTION`, `PRODUCTION_EU`, `SANDBOX`, `SANDBOX_EU`) have changed from `tryvital.io` domains to `junction.com` domains. No code change is required, but any hardcoded URL overrides should be updated accordingly.
### Added
* **`MealInDbBaseClientFacingSource.getCalendarDate()`** — returns the date of the meal in `YYYY-MM-DD` format, supporting providers that only expose a calendar date.

## 0.0.1 - 2026-05-01
* Initial SDK generation
* 🌿 Generated with Fern

