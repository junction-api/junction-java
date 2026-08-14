## 1.3.0 - 2026-08-14

### Added

* **Orderable-test search** — added sync and async `CompendiumClient.searchOrderableTests()` methods and the related request and response models.
* **Unmatched lab-result management** — added sync and async methods for listing, testing, reviewing, accepting, and resolving unmatched results, together with match-review webhook models.
* **Lab-test pricing** — added pricing models and optional `includePricing` and `labAccountId` request fields.
* **Provider and lab coverage** — added Google Health provider and OAuth values and the MTL lab value.
* **Lab metadata** — added optional source interpretation, lab logo URL, and lab-location website fields.

### Changed

* **HTTP reliability** — added configurable retry jitter, per-request retry overrides, and decompression for encoded responses.

### Beta

* **Aggregate and lab-report states** — added the result-table resource and processing-error parsing state without affecting the stable-surface SemVer calculation.

## 1.2.0 - 2026-06-05
### Added
* **`AlignExpr`** — new public symbol
* **`AlignExprCarry`** — new public symbol
* **`CarryBackwardExpr`** — new public symbol
* **`CarryForwardExpr`** — new public symbol
* **`CarryNearestExpr`** — new public symbol
### Changed
* **`Query`** — new optional field(s): align
### Beta
* **`LabReportResult`** — field(s) removed: isSensitive
* **`LabReportResultIsSensitive`** — public symbol removed
* **`LabReportResultSensitivity`** — new public symbol
* **`ParsingJobFailureReason`** — model changed (backwards-compatible)

## 1.1.0 - 2026-05-27
* ## [1.1.0] - 2025
### Added
* **`updateOrder()`** — new method on `LabTestsClient` and `AsyncLabTestsClient` to update a modifiable order's scheduled activation date via a PATCH request to `v3/order/{orderId}`.
* **`UpdateOrderBody`** — new request class with an optional `activate_by` field, supporting `Optional<String>` and `Nullable<String>` builder overloads for clearing or setting the scheduled dispatch date.
* **`PatchOrderCommunicationSettingsBody`** and **`PatchOrderCommunicationSettingsResponse`** — new types for managing order SMS communication settings.
* **`GetOrderCommunicationSettingsResponse`** — new response type exposing `orderId` and `smsEnabled` fields for order communication settings.
* **`LabReportResult.isSensitive`** and **`LabReportResult.loincMatchStatus`** — new optional fields with corresponding enums `LabReportResultIsSensitive` and `LabReportResultLoincMatchStatus` for richer lab result metadata.

## 1.0.1 - 2026-05-07
* fix: fix request field serialization across all request types
* Previously, required fields like `start_date`, `zip_code`, `lab_id`,
* `user_id`, `collection_date`, and `lab` were annotated with `@JsonIgnore`,
* causing them to be omitted from serialized request bodies. Optional fields
* (`end_date`, `provider`, `cursor`, `next_cursor`, etc.) also lacked proper
* `@JsonProperty` bindings and `NullableNonemptyFilter` handling.
* This fix ensures all request fields are correctly serialized when making
* API calls, resolving silent data-loss bugs where required parameters were
* never sent to the server.
* Key changes:
* Replace `@JsonIgnore` with `@JsonProperty` on required fields across all request classes (activity, body, sleep, vitals, lab tests, link, meal, menstrual cycle, etc.)
* Add private `@JsonProperty`-annotated accessors with `NullableNonemptyFilter` for all optional (`Optional<T>`) fields to ensure correct conditional serialization
* Import `NullableNonemptyFilter` and `JsonProperty` in all affected request classes
* 🌿 Generated with Fern

## 1.0.0 - 2026-05-06
* Initial SDK generation
* 🌿 Generated with Fern
