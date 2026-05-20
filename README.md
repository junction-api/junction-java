# Junction Java Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Fjunction-api%2Fjunction-java)
[![Maven Central](https://img.shields.io/maven-central/v/com.junction/junction-java)](https://central.sonatype.com/artifact/com.junction/junction-java)

Power Every Diagnostic Touchpoint – at Scale. Integrate 300+ wearables, streamline lab operations, and build your own lab testing experience spanning all 50 states – with a single API.

## Table of Contents

- [Documentation](#documentation)
- [Installation](#installation)
- [Reference](#reference)
- [Usage](#usage)
- [Environments](#environments)
- [Base Url](#base-url)
- [Exception Handling](#exception-handling)
- [Advanced](#advanced)
  - [Custom Client](#custom-client)
  - [Retries](#retries)
  - [Timeouts](#timeouts)
  - [Custom Headers](#custom-headers)
  - [Access Raw Response Data](#access-raw-response-data)
- [Contributing](#contributing)

## Documentation

API reference documentation is available [here](https://docs.junction.com/).

## Installation

### Gradle

Add the dependency in your `build.gradle` file:

```groovy
dependencies {
  implementation 'com.junction:junction-java:1.0.2'
}
```

### Maven

Add the dependency in your `pom.xml` file:

```xml
<dependency>
  <groupId>com.junction</groupId>
  <artifactId>junction-java</artifactId>
  <version>1.0.2</version>
</dependency>
```

## Reference

A full reference for this library is available [here](https://github.com/junction-api/junction-java/blob/HEAD/./reference.md).

## Usage

Instantiate and use the client with the following:

```java
package com.example.usage;

import com.junction.api.Junction;
import com.junction.api.resources.link.requests.BulkImportConnectionsBody;
import com.junction.api.types.ConnectionRecipe;
import com.junction.api.types.OAuthProviders;
import java.util.Arrays;

public class Example {
    public static void main(String[] args) {
        Junction client = Junction
            .builder()
            .apiKey("<value>")
            .build();

        client.link().bulkImport(
            BulkImportConnectionsBody
                .builder()
                .provider(OAuthProviders.OURA)
                .connections(
                    Arrays.asList(
                        ConnectionRecipe
                            .builder()
                            .userId("user_id")
                            .accessToken("access_token")
                            .refreshToken("refresh_token")
                            .providerId("provider_id")
                            .expiresAt(1)
                            .build()
                    )
                )
                .build()
        );
    }
}
```

## Environments

This SDK allows you to configure different environments for API requests.

```java
import com.junction.api.Junction;
import com.junction.api.core.Environment;

Junction client = Junction
    .builder()
    .environment(Environment.Production)
    .build();
```

## Base Url

You can set a custom base URL when constructing the client.

```java
import com.junction.api.Junction;

Junction client = Junction
    .builder()
    .url("https://example.com")
    .build();
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), an API exception will be thrown.

```java
import com.junction.api.core.ApiError;

try{
    client.link().bulkImport(...);
} catch (ApiError e){
    // Do something with the API exception...
}
```

## Advanced

### Custom Client

This SDK is built to work with any instance of `OkHttpClient`. By default, if no client is provided, the SDK will construct one.
However, you can pass your own client like so:

```java
import com.junction.api.Junction;
import okhttp3.OkHttpClient;

OkHttpClient customClient = ...;

Junction client = Junction
    .builder()
    .httpClient(customClient)
    .build();
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2). Before defaulting to exponential backoff, the SDK will first attempt to respect
the `Retry-After` header (as either in seconds or as an HTTP date), and then the `X-RateLimit-Reset` header
(as a Unix timestamp in epoch seconds); failing both of those, it will fall back to exponential backoff.

Which status codes are retried depends on the `retry-status-codes` generator configuration:

**`legacy`** (current default): retries on
- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (All server errors, including 500)

**`recommended`**: retries on
- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [502](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502) (Bad Gateway)
- [503](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503) (Service Unavailable)
- [504](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504) (Gateway Timeout)

Use the `maxRetries` client option to configure this behavior.

```java
import com.junction.api.Junction;

Junction client = Junction
    .builder()
    .maxRetries(1)
    .build();
```

### Timeouts

The SDK defaults to a 60 second timeout. You can configure this with a timeout option at the client or request level.
```java
import com.junction.api.Junction;
import com.junction.api.core.RequestOptions;

// Client level
Junction client = Junction
    .builder()
    .timeout(60)
    .build();

// Request level
client.link().bulkImport(
    ...,
    RequestOptions
        .builder()
        .timeout(60)
        .build()
);
```

### Custom Headers

The SDK allows you to add custom headers to requests. You can configure headers at the client level or at the request level.

```java
import com.junction.api.Junction;
import com.junction.api.core.RequestOptions;

// Client level
Junction client = Junction
    .builder()
    .addHeader("X-Custom-Header", "custom-value")
    .addHeader("X-Request-Id", "abc-123")
    .build();
;

// Request level
client.link().bulkImport(
    ...,
    RequestOptions
        .builder()
        .addHeader("X-Request-Header", "request-value")
        .build()
);
```

### Access Raw Response Data

The SDK provides access to raw response data, including headers, through the `withRawResponse()` method.
The `withRawResponse()` method returns a raw client that wraps all responses with `body()` and `headers()` methods.
(A normal client's `response` is identical to a raw client's `response.body()`.)

```java
JunctionHttpResponse response = client.link().withRawResponse().bulkImport(...);

System.out.println(response.body());
System.out.println(response.headers().get("X-My-Header"));
```

## Contributing

While we value open-source contributions to this SDK, this library is generated programmatically.
Additions made directly to this library would have to be moved over to our generation code,
otherwise they would be overwritten upon the next generated release. Feel free to open a PR as
a proof of concept, but know that we will not be able to merge it as-is. We suggest opening
an issue first to discuss with us!

On the other hand, contributions to the README are always very welcome!
