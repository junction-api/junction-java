## [3.0.0] - 2026-10-16
### Breaking Changes

- **`GetUnmatchedResultResponse`** — public class removed; any code that references this type, its builder, or its staged builder interfaces (`IdStage`, `StatusStage`, `DecisionCodeStage`, etc.) will fail to compile. Remove usages or migrate to the replacement response type returned by the unmatched-result API.
- **`AsyncLabTestsClient.getUnmatchedResult()`** — all overloads now return `CompletableFuture<UnmatchedResult>` instead of `CompletableFuture<GetUnmatchedResultResponse>`; update call sites to use `UnmatchedResult` and remove any references to `GetUnmatchedResultResponse`.
- **`getUnmatchedResult`** — return type changed from `GetUnmatchedResultResponse` to `UnmatchedResult`; update all call sites to use `UnmatchedResult` instead of `GetUnmatchedResultResponse`.
- **`getUnmatchedResult()`** — return type changed from `GetUnmatchedResultResponse` to `UnmatchedResult` across all overloads; update variable declarations and assignments to use `UnmatchedResult`.
- **`getUnmatchedResult()`** — all overloads now return `JunctionHttpResponse<UnmatchedResult>` instead of `JunctionHttpResponse<GetUnmatchedResultResponse>`; update call sites to use `UnmatchedResult` and remove any references to `GetUnmatchedResultResponse`.
- **`UnmatchedResult`** — `getIsStale()` and the `isStale()` builder methods have been removed. Remove any calls to these methods; there is no direct replacement.

### Added

- **`AsyncLabTestsClient.getVersionHistory()`** — new async methods to list immutable marker-set versions for a lab test.
- **`AsyncLabTestsClient.listPromotions()` and `getPromotionSource()`** — new async methods for retrieving lab-test promotions and their sources.
- **`AsyncLabTestsClient.getLabTestCollectionInstructions()`** — new async methods to retrieve tube count and collection-instructions PDF for at-home phlebotomy lab tests.
- **`AsyncLabTestsClient.listUnmatchedResultUpdates()` and `createUnmatchedResultUpdate()`** — new async methods for managing unmatched result updates, plus an optional `appointment` field on `CreateCheckoutSessionBody`.
- **`getVersionHistory()`** — retrieves the list of immutable marker-set versions for a given lab test.
- **`listPromotions()` and `getPromotionSource()`** — new methods for querying lab-test promotions and their source information.
- **`getLabTestCollectionInstructions()`** — retrieves tube count and collection-instructions data for at-home phlebotomy lab tests.
- **`listUnmatchedResultUpdates()` and `createUnmatchedResultUpdate()`** — new methods for listing and creating updates on unmatched lab results.
- **`getVersionHistory()`** — new overloads on `LabTestsClient` to list immutable marker-set versions for a given lab test.
- **`listPromotions()` and `getPromotionSource()`** — new overloads to retrieve lab-test promotions and their promotion sources.
- **`getLabTestCollectionInstructions()`** — new overloads to fetch tube counts and collection-instruction PDFs for at-home phlebotomy lab tests.
- **`listUnmatchedResultUpdates()` and `createUnmatchedResultUpdate()`** — new overloads for listing and creating updates on unmatched lab results.
- **`getVersionHistory()`** — new `RawLabTestsClient` methods to list immutable marker-set versions for a given lab test, with optional pagination via `GetVersionHistoryLabTestsRequest`.
- **`listPromotions()` and `getPromotionSource()`** — new methods for retrieving lab-test promotions and their promotion source, returning `LabTestPromotion` and `LabTestPromotionSource` respectively.
- **`getLabTestCollectionInstructions()`** — new methods to fetch tube count and collection instructions for at-home phlebotomy lab tests, returning `GetLabTestCollectionInstructionsResponse`.
- **`listUnmatchedResultUpdates()` and `createUnmatchedResultUpdate()`** — new methods for managing updates on unmatched results, with `ListUnmatchedResultUpdatesResponse` and `UnmatchedResult` return types.
- **`GetByIdLabTestsRequest.version`** — new optional `Integer` field to pin a specific immutable marker-set version when fetching a lab test by ID.
- **`UnmatchedResult`** — new optional fields `latestActivityActorId`, `latestActivityActorType`, `latestActivityAt`, `accessionNumbers`, and `rawResultCollectionDates` for richer activity and lab-result metadata.
- **`UpdateLabTestRequest`** — new optional builder fields `labAccountId`, `markerIds`, and `providerIds` to control lab account association and panel membership when updating a lab test.
- **`ClientFacingMarkerComplete`** — new optional `components` field (`List<ClientFacingMarkerComplete>`) exposing recursive panel component markers.
- **`OrderSetRequest`** — new optional `parameters` field (`OrderSetParameters`) for supplying order-set parameters at request time.
- **`CreateUnmatchedResultUpdateBody`** — new request class for creating unmatched lab-result updates, with optional `note` and `status` fields.
- **`ListUnmatchedResultUpdatesLabTestsRequest`** — new paginated request class for listing unmatched result updates, supporting `limit` and `nextCursor`.
- **`GetPromotionSourceLabTestsRequest`**, **`ListPromotionsLabTestsRequest`**, **`GetVersionHistoryLabTestsRequest`** — new request classes for lab-test promotion-source lookup, promotion listing (with optional `sourceSandboxLabTestIds` filter), and cursor-paginated version history.
- **`GetLabTestCollectionInstructionsLabTestsRequest`** — new request class for the lab-test collection-instructions endpoint.
- **`CheckoutSessionAppointment`** — new model representing a PSC appointment attached to a checkout session, with `bookingKey`, `appointmentNotes`, and optional `asyncConfirmationTimeoutMillisecond` fields.
- **`GetLabTestCollectionInstructionsResponse`** — new response model exposing `totalTubes` and `lab` for lab-test collection instructions.
- **`GetLabTestVersionHistoryResponse`** — new paginated response model for lab-test version history, containing a list of `LabTestVersionSummary` and an optional `nextCursor`.
- **`ClientFacingOrder.labAccountId`** — new optional `labAccountId` field on orders, accessible via `getLabAccountId()` and settable through the builder with `Optional<String>`, `String`, or `Nullable<String>` overloads.
- **`ClientFacingSleep` sleep-stage seconds** — five new optional fields (`stageAsleepSecond`, `stageAwakeSecond`, `stageLightSecond`, `stageRemSecond`, `stageDeepSecond`) providing per-stage duration in seconds.
- **`LabTestPromotion`** — new model representing a lab test promotion from sandbox to production, with source sandbox and production lab test ID fields.
- **`LabTestPromotionSource`** — new model capturing lab test metadata (name, description, collection method, fasting, lab slug, provider IDs) used as the source for a promotion.
- **`LabTestVersionSummary`** and **`LabTestVersionMarkerSummary`** — new models for tracking lab test version history and associated markers.
- **`ListUnmatchedResultUpdatesResponse`** — new paginated response model for listing unmatched result updates, with optional `data` and `nextCursor` fields.
- **`MatchReviewStatusFilter.PENDING_CUSTOMER_REVIEW_IN_PROGRESS`** — new filter constant, enum value, and `visitPendingCustomerReviewInProgress()` visitor method added to `MatchReviewStatusFilter`.
- **`MatchReviewTransitionStatus`** — new enum representing the full set of match-review lifecycle states (`PENDING_CUSTOMER_REVIEW`, `PENDING_CUSTOMER_REVIEW_IN_PROGRESS`, `RESOLVED_REJECTED`, `RESOLVED_ACCEPTED`, `PENDING_OPS_REVIEW`, `MATCHED`).
- **`UnmatchedResultUpdate`** — new model capturing a status-transition event on an unmatched lab result, including `fromStatus`, `toStatus`, actor details, an optional note, and a `createdAt` timestamp.
- **`UnmatchedResultUpdateActorType`** and **`UnmatchedResultUpdateStatus`** — new enums for classifying the actor (`CUSTOMER`, `API`) and pending-review status of an unmatched-result update.
- **`UnmatchedResultLatestActivityActorType`** — new enum (`CUSTOMER`, `API`) for identifying the actor on the latest activity of an unmatched result.
- **`OrderSetFastingRequirement`** and **`OrderSetParameters`** — new enum and model for specifying fasting requirements (`FASTING_REQUIRED`, `FASTING_NOT_REQUIRED`) on an order set.

### Changed

- **`CheckoutSession`** — `getPaymentResourceId()`, `getPaymentResourceUrl()`, and `getPaymentResourceClientSecret()` now carry detailed Javadoc describing their Stripe semantics and nullability rules.
- **`ClientFacingHrvTimeseries`** — `getUnit()` and `getValue()` Javadoc updated to reflect that the HRV method is `sdnn` for Apple HealthKit and `rmssd` for other providers, with values in milliseconds.

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
