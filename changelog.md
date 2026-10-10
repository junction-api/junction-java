## [3.0.0] - 2026-10-10
### Breaking Changes
- **`GetUnmatchedResultResponse`** — this public class and its entire staged builder (`IdStage`, `StatusStage`, `DecisionCodeStage`, `ReasonStage`, `LabStage`, `CreatedAtStage`, `UpdatedAtStage`, `_FinalStage`) have been removed. Callers must migrate to the replacement response type returned by the unmatched-result API.

### Breaking Changes
- **`getUnmatchedResult()`** — return type changed from `GetUnmatchedResultResponse` to `UnmatchedResult` on both sync and async clients; update any code that references `GetUnmatchedResultResponse` to use `UnmatchedResult` instead.

### Added
- **`listPromotions()`** and **`getPromotionSource()`** — new async methods on `AsyncLabTestsClient` for listing available lab test promotions and retrieving the promotion source for a specific lab test.
- **`getLabTestCollectionInstructions()`** — new async method on `AsyncLabTestsClient` that returns tube count and collection instructions for at-home phlebotomy lab tests (requires `enable_approxdraw_labcorp` or `enable_approxdraw` feature flags).
- **`listUnmatchedResultUpdates()`** and **`createUnmatchedResultUpdate()`** — new async methods on `AsyncLabTestsClient` for managing updates to unmatched lab results.
- **`CreateCheckoutSessionBody.appointment`** — new optional `CheckoutSessionAppointment` field with full builder support (`appointment(Optional)`, `appointment(CheckoutSessionAppointment)`, `appointment(Nullable)`).

### Breaking Changes
- **`getUnmatchedResult()`** — return type changed from `JunctionHttpResponse<GetUnmatchedResultResponse>` to `JunctionHttpResponse<UnmatchedResult>`; update call sites to use `UnmatchedResult` directly and remove any references to `GetUnmatchedResultResponse`.

### Added
- **`listPromotions()`** and **`getPromotionSource()`** — new async methods on `AsyncRawLabTestsClient` for retrieving lab-test promotion lists and per-test promotion source details, backed by `LabTestPromotion` and `LabTestPromotionSource` models.
- **`getLabTestCollectionInstructions()`** — new async method that retrieves tube count and collection-instructions data for at-home phlebotomy lab tests, returning `GetLabTestCollectionInstructionsResponse`.
- **`listUnmatchedResultUpdates()`** and **`createUnmatchedResultUpdate()`** — new async methods for paginating and posting updates to an unmatched result, returning `ListUnmatchedResultUpdatesResponse` and `UnmatchedResult` respectively.

### Breaking Changes
- **`getUnmatchedResult()`** now returns `UnmatchedResult` instead of `GetUnmatchedResultResponse`. Update any code that references `GetUnmatchedResultResponse` to use `UnmatchedResult` instead.

### Added
- **`listPromotions()`** and **`getPromotionSource()`** — new `LabTestsClient` methods for listing lab test promotions and retrieving the promotion source for a given lab test.
- **`getLabTestCollectionInstructions()`** — new `LabTestsClient` overloads to retrieve tube count and collection instructions for at-home phlebotomy lab tests (requires `enable_approxdraw_labcorp` or `enable_approxdraw` feature flags).
- **`listUnmatchedResultUpdates()`** and **`createUnmatchedResultUpdate()`** — new `LabTestsClient` overloads for managing updates to unmatched lab results.
- **New types** — `LabTestPromotion`, `LabTestPromotionSource`, `GetLabTestCollectionInstructionsResponse`, `ListUnmatchedResultUpdatesResponse`, and associated request classes added to support the new methods.

### Breaking Changes
- **`getUnmatchedResult()`** now returns `JunctionHttpResponse<UnmatchedResult>` instead of `JunctionHttpResponse<GetUnmatchedResultResponse>`; update call sites to use `UnmatchedResult` and remove any references to `GetUnmatchedResultResponse`.

### Added
- **`listPromotions()`** and **`getPromotionSource()`** — new methods on the lab-tests client for retrieving lab-test promotions and their sources, backed by `LabTestPromotion`, `LabTestPromotionSource`, `ListPromotionsLabTestsRequest`, and `GetPromotionSourceLabTestsRequest`.
- **`getLabTestCollectionInstructions()`** — retrieves tube count and collection-instructions data for an at-home phlebotomy lab test, returning `GetLabTestCollectionInstructionsResponse`.
- **`listUnmatchedResultUpdates()`** and **`createUnmatchedResultUpdate()`** — new methods for paginating and posting updates to an unmatched result, with `ListUnmatchedResultUpdatesResponse` and `CreateUnmatchedResultUpdateBody`.
- **`OrderSetRequest.parameters`** — new optional `OrderSetParameters` field on `OrderSetRequest` with full builder support.

### Breaking Changes
- **`UnmatchedResult`** — `getIsStale()` and the `isStale(Boolean)` / `isStale(Optional<Boolean>)` builder methods have been removed. Remove any calls to these methods; the field is no longer part of the model.

### Added
- **`UnmatchedResult`** — three new optional fields track the latest activity on an unmatched result: `getLatestActivityActorId()`, `getLatestActivityActorType()` (returns `UnmatchedResultLatestActivityActorType`), and `getLatestActivityAt()`, each with corresponding builder overloads.
- **`CreateUnmatchedResultUpdateBody`** — new request class for submitting unmatched-result status updates, with optional `note` and `status` (`UnmatchedResultUpdateStatus`) fields.
- **`ListPromotionsLabTestsRequest`**, **`GetPromotionSourceLabTestsRequest`**, and **`GetLabTestCollectionInstructionsLabTestsRequest`** — new request classes supporting lab-test promotion endpoints.

### Changed
- **`ListUnmatchedResultsLabTestsRequest`** — the `status` filter now documents the `pending_customer_review:in_progress` value, which returns items your team is actively working on.
- **`ClientFacingHrvTimeseries`** — `unit` and `value` documentation updated to clarify that HRV values are in milliseconds and that Apple HealthKit reports sdnn while other providers report rmssd.

### Added
- **`ListUnmatchedResultUpdatesLabTestsRequest`** — new paginated request class with optional `limit` and `nextCursor` fields for listing unmatched result updates.
- **`CheckoutSessionAppointment`** — new model for attaching a PSC appointment booking to a checkout session, with required `bookingKey` and `appointmentNotes` and optional `asyncConfirmationTimeoutMillisecond`.
- **`ClientFacingOrder.getLabAccountId()`** — new optional `labAccountId` field (and corresponding builder overloads) on order responses to surface the associated lab account.
- **`CheckoutSession`** — enriched Javadoc for `paymentResourceId`, `paymentResourceUrl`, and `paymentResourceClientSecret` fields clarifying Stripe resource semantics and webhook nullability.

### Added

- **`ClientFacingSleep`** — five new optional sleep stage duration fields: `stageAsleepSecond`, `stageAwakeSecond`, `stageLightSecond`, `stageRemSecond`, and `stageDeepSecond`, each exposed as `Optional<Integer>` with full builder support.
- **`GetLabTestCollectionInstructionsResponse`** — new response type returning `totalTubes` and a `ClientFacingLab` for lab test collection instructions.
- **`LabTestPromotion`** — new model representing a sandbox-to-production lab test promotion, carrying source sandbox ID, production ID, and status.
- **`LabTestPromotionSource`** — new model capturing the full metadata (name, description, collection method, fasting, lab slug, provider IDs, and source sandbox ID) of a lab test being promoted.

### Added
- **`UnmatchedResultUpdate`** — new model representing a single status-transition event on an unmatched lab result, including `fromStatus`, `toStatus`, `actorType`, `actorId`, `note`, and `createdAt`.
- **`ListUnmatchedResultUpdatesResponse`** — paginated response wrapper (`data`, `nextCursor`) for listing unmatched result update history.
- **`MatchReviewTransitionStatus`** — new enum covering all review lifecycle states: `pending_customer_review`, `pending_customer_review:in_progress`, `resolved:accepted`, `resolved:rejected`, `pending_ops_review`, and `matched`.
- **`MatchReviewStatusFilter.PENDING_CUSTOMER_REVIEW_IN_PROGRESS`** — new filter value and corresponding `Visitor.visitPendingCustomerReviewInProgress()` method added to the existing filter enum.
- **`OrderSetFastingRequirement`** and **`OrderSetParameters`** — new enum and model for specifying fasting requirements (`fasting_required`, `fasting_not_required`) when working with order sets.

### Added
- **`UnmatchedResultUpdateStatus`** — new enum type representing the review lifecycle of unmatched lab results, with values `PENDING_CUSTOMER_REVIEW` and `PENDING_CUSTOMER_REVIEW_IN_PROGRESS` and a `Visitor<T>` interface for exhaustive handling.

## 2.0.0 - 2026-09-24

### Added

* **Order tracking** — added sync and async `getOrderTracking()` methods, tracking models, and an order-tracking webhook model.
* **Test-kit idempotency** — added optional idempotency controls when creating a test-kit order.
* **Result and status details** — added stale-result indicators and expanded order-status values.
* **Horizon AI device reliability** — added reliability columns for query selection, grouping, and aggregate expressions.

### Changed

* **Pricing conditions** — `PricingModifierMarkerPricingConditions.keys` is now required; callers building this model must supply it.

### Removed

* **Legacy timeseries methods** — removed the non-grouped `vitals` methods (including `steps()` and `heartrate()`) and their request classes. Use the corresponding `*Grouped()` methods and handle their paginated grouped responses.
* **Hypnogram timeseries** — removed `hypnogram()`, `hypnogramGrouped()`, their models, and the sleep-stream hypnogram field. Use sleep-cycle summaries and events.
* **Deprecated event fields** — removed `ClientFacingSource.name`, `logo`, and `slug`; `ProviderConnectionCreated.source`; and `HistoricalPullCompleted.isFinal`. Use source context, `ProviderConnectionCreated.provider`, and the completed event itself.

### Beta

* **Checkout** — added sync and async clients for quotes and checkout sessions, with checkout webhook models and the `upfront_payment` billing value.
* **Order-set pricing estimates** — added sync and async `estimateOrderSetPricing()` methods and pricing models.

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
