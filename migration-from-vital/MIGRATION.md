# Migration guide

_From `tryVital/vital-java@1.2.588` to the new release._

## Install the new package

<a id="install"></a>

This is a *replacement* dependency, not a version bump. Update your `build.gradle` (or `pom.xml`) to drop the old coordinate and add the new one, then re-resolve.

After switching the dependency, every `import com.vital...` line needs to become `import com.junction...` (covered by the package-prefix update below).

Things to look out for:

- Aliased dependency declarations in `libs.versions.toml` / `gradle/libs.versions.toml` catalogs.
- Internal repackaged or shaded JARs that bundle the old artifact.
- CI artifact-cache pins, Dockerfile `COPY` lines, and lockfiles (`gradle.lockfile`).
- Documentation, READMEs, and onboarding scripts that quote the old Maven coordinate as a literal string.

**Before:**

```groovy
// build.gradle
dependencies {
    implementation 'io.tryvital:vital-java:1.2.605'
}
```

```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.tryvital</groupId>
    <artifactId>vital-java</artifactId>
    <version>1.2.605</version>
</dependency>
```

**After:**

```groovy
// build.gradle
dependencies {
    implementation 'com.junction:junction-java:1.0.0'
}
```

```xml
<!-- pom.xml -->
<dependency>
    <groupId>com.junction</groupId>
    <artifactId>junction-java</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Pick up the new environment URLs

<a id="rename-environment-enum"></a>

If your code constructs the client with `Environment.PRODUCTION` (or `PRODUCTION_EU`, `SANDBOX`, `SANDBOX_EU`), no change is needed — the constants now point at the new hosts. If your code passes a literal URL or compares response/log strings against an old URL, update each one using the table below:

| Environment       | Before                                | After                                   |
|-------------------|---------------------------------------|-----------------------------------------|
| Production (US)   | `https://api.tryvital.io`             | `https://api.us.junction.com`           |
| Production (EU)   | `https://api.eu.tryvital.io`          | `https://api.eu.junction.com`           |
| Sandbox (US)      | `https://api.sandbox.tryvital.io`     | `https://api.sandbox.us.junction.com`   |
| Sandbox (EU)      | `https://api.sandbox.eu.tryvital.io`  | `https://api.sandbox.eu.junction.com`   |

Things to look out for:

- Outbound-allowlist firewall rules and proxy configurations pinned to the old hostnames.
- Mocked HTTP fixtures (WireMock, MockServer) that reference the old URLs.
- Environment variables that pin a base URL (e.g. `VITAL_BASE_URL`).

**Before:**

```java
Vital client = Vital.builder()
    .apiKey("...")
    .url("https://api.tryvital.io")
    .build();
```

**After:**

```java
Junction client = Junction.builder()
    .apiKey("...")
    .url("https://api.us.junction.com")
    .build();
```

## Update exception and HTTP-response references

<a id="rename-exception-types"></a>

Update `catch` clauses, `throws` declarations, and any helper-method signatures or generic bounds that mention `VitalException` or `VitalHttpResponse` to use `JunctionException` and `JunctionHttpResponse` respectively.

**Before:**

```java
try {
    client.user().get(userId);
} catch (VitalException e) {
    log.error("call failed", e);
}

VitalHttpResponse<User> raw = client.user().withRawResponse().get(userId);
```

**After:**

```java
try {
    client.user().get(userId);
} catch (JunctionException e) {
    log.error("call failed", e);
}

JunctionHttpResponse<User> raw = client.user().withRawResponse().get(userId);
```

## Update import paths

<a id="rename-package-prefix"></a>

Replace every `import com.vital...` line in your codebase with `import com.junction...`. The sub-package layout under the new prefix is identical, so only the prefix changes.

Things to look out for:

- Fully-qualified class names embedded in strings (reflection, Spring config, logging configs).
- IDE-generated suppression comments referencing the old package.
- `package-info.java` files in any module that wraps or extends SDK types.

**Before:**

```java
import com.vital.api.Vital;
import com.vital.api.resources.user.UserClient;
import com.vital.api.resources.user.requests.UserCreateBody;
```

**After:**

```java
import com.junction.api.Junction;
import com.junction.api.resources.user.UserClient;
import com.junction.api.resources.user.requests.UserCreateBody;
```

## Update client class names

<a id="rename-client-class"></a>

The entry-point client classes have been renamed across the SDK:

- `Vital` → `Junction`
- `VitalBuilder` → `JunctionBuilder`
- `AsyncVital` → `AsyncJunction`
- `AsyncVitalBuilder` → `AsyncJunctionBuilder`

Things to look out for:

- Field declarations and method-parameter types that hold a typed client reference.
- Test fixtures and DI / Spring beans that wire the client by type.
- Import statements (covered separately by the package-prefix update).

**Before:**

```java
import com.vital.api.Vital;

Vital client = Vital.builder()
    .apiKey("...")
    .build();
```

**After:**

```java
import com.junction.api.Junction;

Junction client = Junction.builder()
    .apiKey("...")
    .build();
```

## Pass path parameters as positional arguments

<a id="inline-path-parameters"></a>

For each endpoint with a path parameter:

- Drop the wrapper request builder and pass the path values directly as method arguments, in the order they appear in the URL template.
- If the endpoint also takes body or query parameters, those still go through a request object — pass it as the next argument after the path values.
- The wrapper request types that previously held only path params are no longer generated; types that hold body/query params remain (with their new verb-first names — see the request-type rename section below).

**Before:**

```java
client.user().get(
    UserGetRequest.builder()
        .userId(userId)
        .build()
);
```

**After:**

```java
client.user().get(userId);
```

## Update request-type imports to verb-first names

<a id="request-type-rename"></a>

Each per-endpoint request type is renamed so the verb leads. The mechanical rule is: the resource name moves from the front of the class to a suffix. A few representative renames:

- `LabTestsGetMarkersRequest` → `GetMarkersLabTestsRequest`
- `LinkListBulkOpsRequest` → `ListBulkOpsLinkRequest`
- `LinkGetAllProvidersRequest` → `GetAllProvidersLinkRequest`

If your code only constructs request objects inline at the call site, this change is invisible. If you import the type by name (for helper-method signatures, custom factories, or annotations), update each reference.

**Before:**

```java
import com.vital.api.resources.labtests.requests.LabTestsGetMarkersRequest;

LabTestsGetMarkersRequest req = LabTestsGetMarkersRequest.builder()
    .labId(labId)
    .build();
client.labTests().getMarkers(req);
```

**After:**

```java
import com.junction.api.resources.labtests.requests.GetMarkersLabTestsRequest;

GetMarkersLabTestsRequest req = GetMarkersLabTestsRequest.builder()
    .labId(labId)
    .build();
client.labTests().getMarkers(req);
```

## Unwrap nullable getters

<a id="respect-nullable-schemas"></a>

For each model field that the API documents as nullable, the getter now returns `Optional<T>`. Update call sites to unwrap explicitly with `.orElse(default)`, `.isPresent()` / `.get()`, or `.ifPresent(...)` depending on how you want to handle missing values.

Builder setters for the same fields accept `null` (treated as absent) and an `Optional<T>` overload. If you set a previously non-null field with `null`, the builder no longer rejects it; verify the resulting payload still matches the API contract.

**Before:**

```java
ClientFacingUser user = client.user().get(userId);
String email = user.getEmail();
```

**After:**

```java
ClientFacingUser user = client.user().get(userId);
String email = user.getEmail().orElse(null);
```

## Handle the unknown-enum sentinel in switch statements

<a id="forward-compatible-enums"></a>

Any `switch` over an enum's `getEnumValue()` should include either a `default` arm or an explicit `_UNKNOWN` case so unrecognised wire values are handled instead of silently falling through. Pattern-matching `instanceof` and equality checks against unknown values now compare to the sentinel rather than throwing.

**Before:**

```java
switch (source.getProvider().getEnumValue()) {
    case OURA:
        handleOura();
        break;
    case FITBIT:
        handleFitbit();
        break;
}
```

**After:**

```java
switch (source.getProvider().getEnumValue()) {
    case OURA:
        handleOura();
        break;
    case FITBIT:
        handleFitbit();
        break;
    case _UNKNOWN:
    default:
        handleUnknown(source.getProvider().toString());
        break;
}
```

## Drop calls to the legacy link methods

<a id="link-removed-legacy-endpoints"></a>

These methods were deprecated for years and were never part of the documented Junction Link integration. Customer code that didn't reference them needs no migration. If you do have call sites, replace them with the Junction Link integration described in the API reference — there isn't a one-to-one drop-in, the legacy methods wrapped retired endpoints with no published equivalents.

## Drop imports for removed public types

<a id="public-types-removed"></a>

Three names were exported from the package root in the old SDK but are not present in the new SDK:

- `AuthType` — was only used by the legacy `emailAuth` flow.
- `ManualProviders` — was the parameter type for the now-removed `connectManualProvider` method.
- `ClientFacingUserKey` — callers should switch to `ClientFacingUser`. The endpoint that previously returned `ClientFacingUserKey` now returns `ClientFacingUser`.

**Before:**

```java
import com.vital.api.types.AuthType;
import com.vital.api.types.ManualProviders;
import com.vital.api.types.ClientFacingUserKey;
```

**After:**

```java
// AuthType and ManualProviders have no replacement — drop the imports.
// ClientFacingUserKey -> ClientFacingUser
import com.junction.api.types.ClientFacingUser;
```
