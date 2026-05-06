# Changelog

_Major release. Baseline: `tryVital/vital-java@1.2.588`._

## Breaking changes

- **Maven coordinate renamed** — [migration steps](MIGRATION.md#install)
  The Java SDK is published under a new Maven coordinate:

  - `io.tryvital:vital-java` → `com.junction:junction-java`

  The old artifact will not receive further releases — replace your dependency declaration and re-resolve. This is a coordinate replacement, not a version bump: it ships alongside the package-prefix and class renames, so an in-place version update will not compile.
- **Environment URLs moved to junction.com hosts** — [migration steps](MIGRATION.md#rename-environment-enum)
  Every base URL exposed by the SDK has been replaced as part of the rebrand:

  - `https://api.tryvital.io` → `https://api.us.junction.com`
  - `https://api.eu.tryvital.io` → `https://api.eu.junction.com`
  - `https://api.sandbox.tryvital.io` → `https://api.sandbox.us.junction.com`
  - `https://api.sandbox.eu.tryvital.io` → `https://api.sandbox.eu.junction.com`

  Callers that pass `Environment.PRODUCTION` (or the other constants) pick up the new URL automatically because the constant value changed; callers that pass a literal URL string — or that compare against the old URL — must update those references. The `Environment` class name itself is unchanged.
- **Exception and HTTP-response types renamed** — [migration steps](MIGRATION.md#rename-exception-types)
  Two SDK-level types have been renamed:

  - `VitalException` → `JunctionException` (the top-level exception thrown by every SDK call)
  - `VitalHttpResponse` → `JunctionHttpResponse` (the wrapper returned by raw-response variants)

  Update `catch` clauses and any helper signatures that mention these types.
- **Java package path moved to `com.junction.api`** — [migration steps](MIGRATION.md#rename-package-prefix)
  Every generated type now lives under the `com.junction.api` package tree instead of `com.vital.api`. Every `import com.vital...` statement in your codebase needs to become `import com.junction...` — this affects models, request/response types, errors, and the client itself.
- **Top-level client class renamed** — [migration steps](MIGRATION.md#rename-client-class)
  The entry-point client classes have been renamed across the SDK:

  - `Vital` → `Junction`
  - `VitalBuilder` → `JunctionBuilder`
  - `AsyncVital` → `AsyncJunction`
  - `AsyncVitalBuilder` → `AsyncJunctionBuilder`

  Update construction sites and any helper methods or fields that hold a typed client reference.
- **Path parameters are now positional arguments** — [migration steps](MIGRATION.md#inline-path-parameters)
  Every endpoint that takes a path parameter accepts it as a positional argument on the method instead of nested inside a request-object builder. For example, `client.user().get(userId)` replaces the old pattern that built a `UserGetRequest` to hold the `userId`. Body and query parameters are unchanged: pass them as the second argument when present.
- **Per-endpoint request types renamed verb-first** — [migration steps](MIGRATION.md#request-type-rename)
  Auto-generated request types now read fluently from the verb. For example:

  - `LabTestsGetMarkersRequest` → `GetMarkersLabTestsRequest`
  - `LinkListBulkOpsRequest` → `ListBulkOpsLinkRequest`

  The same rule applies to every per-endpoint request class. If you import these types directly (for type annotations on helper methods, custom wiring, or builder reuse), update each import and reference. Most callers that just inline-build the request at the call site are unaffected.
- **Nullable model fields now surface as `Optional`** — [migration steps](MIGRATION.md#respect-nullable-schemas)
  Model getters for fields that the API documents as nullable now return `Optional<T>` (and the matching builder setters accept `null` or an empty `Optional`). Code that previously dereferenced these getters directly will not compile until you unwrap with `.orElse(...)`, `.isPresent()`, or `.get()`. The change is an accuracy fix — the API was always free to return `null` for these fields — so unwrap with a default that matches your previous expectation.

## New & improved

- **New top-level resources**
  The SDK now exposes the `compendium`, `labaccount`, `ordertransaction` resource clients on the root client. See the API reference for the full endpoint list.
- **Pluggable logging and SSE parsing in core**
  The core package now exposes a pluggable `Logger` interface (with a default `ConsoleLogger`), a `LogConfig` you can wire via the client builder, and a built-in `LoggingInterceptor` for request/response logging. Server-sent-events responses can be parsed with the new `SseEventParser` / `SseEvent` helpers. RFC-2822 timestamps deserialise correctly through the new `Rfc2822DateTimeDeserializer`.
- **List physicians on a team**
  `TeamClient` now exposes `getPhysicians(GetPhysiciansTeamRequest)` for listing physicians associated with a team — see the API reference for filter details.
- **Raw response shapes formalized — `*V2InDB` types are now `*Raw`**
  Endpoints whose responses were previously typed as `*V2InDB` now use `*Raw` types. The values are unchanged; only the type names differ:
  
  - `ActivityV2InDB` → `RawActivity`
  - `BodyV2InDB` → `RawBody`
  - `DeviceV2InDB` → `RawDevices`
  - `SleepV2InDB` → `RawSleep`
  - `WorkoutV2InDB` → `RawWorkout`
  
  If you imported one of the old names directly, switch to the new `Raw*` type.
- **Enums tolerate unknown values via a sentinel** — [migration steps](MIGRATION.md#forward-compatible-enums)
  Every generated enum is now forward-compatible. Deserialising a wire value the SDK doesn't recognise no longer throws — it produces an enum instance whose internal value is the sentinel `_UNKNOWN`. This means future server-side enum additions stop being a hard break for older client versions. For example, an unknown `Providers` value now yields a `Providers` instance whose `getEnumValue()` returns `Providers.Value._UNKNOWN`, while `toString()` preserves the raw wire string.
  
  If your code uses a `switch` statement on an enum's `getEnumValue()`, add a `default` (or explicit `_UNKNOWN`) arm so unrecognised values are handled. Equality checks now compare against the sentinel rather than throwing.

## Removed

- **Legacy link methods removed** — [migration steps](MIGRATION.md#link-removed-legacy-endpoints)
  These methods have been deprecated for over two years and were never part of the documented Junction Link integration path; they should not have been in the SDK in the first place. They are removed in this release:

  - `link.tokenState(...)`
  - `link.isTokenValid(...)`
  - `link.startConnect(...)`
  - `link.emailAuth(...)`
  - `link.passwordAuth(...)`
  - `link.connectManualProvider(...)`
  - `team.getLinkConfig(...)`

  The supporting request/body classes that backed these methods are removed alongside them:

  - `BeginLinkTokenRequest`
  - `EmailAuthLink`
  - `PasswordAuthLink`
  - `LinkTokenStateRequest`
  - `LinkTokenValidationRequest`
  - `ManualConnectionData`

  If you have call sites referencing these methods, see the Junction Link API reference for the documented integration path.
- **Public types removed** — [migration steps](MIGRATION.md#public-types-removed)
  Three top-level public types were removed from the SDK with no equivalent. If you imported any of these, drop the import:
  
  - `AuthType` — was used by the legacy `email_auth` flow only.
  - `ManualProviders` — was the parameter type for the now-removed `connectManualProvider` method (one of the legacy link methods, see above). With that method gone, this enum is removed too.
  - `ClientFacingUserKey` — the endpoint that used to return this model now returns `ClientFacingUser` instead. If you depended on the user-key shape, switch to reading the user fields off `ClientFacingUser` directly.
