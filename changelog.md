## 1.0.0 - 2026-05-04
### Breaking Changes
* **`MealInDbBaseClientFacingSource` builder** — the `TimestampStage.timestamp()` method now returns `CalendarDateStage` instead of `NameStage`; existing builder chains must insert a `.calendarDate(...)` call between `.timestamp(...)` and `.name(...)`.
### Added
* **`MealInDbBaseClientFacingSource.getCalendarDate()`** — new required field exposing the meal date in `YYYY-MM-dd` format, sourced from the `calendar_date` JSON property.

## 0.0.1 - 2026-05-01
* Initial SDK generation
* 🌿 Generated with Fern

