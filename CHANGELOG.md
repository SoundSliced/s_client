## 2.2.2
- `s_packages` dependency upgraded to ^1.5.3
- **`s_client` sub-package improvements**:
  - Added `autoRedirectStatusCodes` parameter to `put()`, `putJson()`, and `_performPut()` — enables manual redirect handling for PUT, POST requests, automatically following the `Location` header with a GET request when the response status code matches (consistent with existing POST redirect behavior)
  - Fixed `maxRedirects` guard in `_performPut()` — now only set when `followRedirects` is enabled (matching POST behavior)
  - Stripped Dio `BaseOptions` down to only `baseUrl` and `validateStatus` — all other configuration (`connectTimeout`, `receiveTimeout`, `sendTimeout`, `headers`, `followRedirects`, `maxRedirects`) is now applied per-request via `dio.Options`, avoiding web-specific XHR issues (e.g. `connectTimeout` setting `xhr.timeout`, default `Content-Type` triggering CORS preflights)
  - `connectTimeout` and `sendTimeout` are now forwarded to every `_perform*` method (GET, POST, PUT, PATCH, DELETE, HEAD, download, downloadToFile, uploadFile) — previously only `receiveTimeout` was passed through
  - Explicitly forwarded `Content-Type` from request headers to `dio.Options.contentType` in POST, PUT, and PATCH — ensures Dio's request transformer uses the correct encoder (e.g. form-urlencoded vs JSON) regardless of `BaseOptions` defaults
  - Changed `ClientConfig.connectTimeout`, `receiveTimeout`, and `sendTimeout` defaults from `Duration(seconds: 30)` to `null` (no timeout)
  - Added `_withTimeout<T>()` helper — applies `.timeout()` only when the duration is non-null, replacing all inline `.timeout()` calls on `http` package requests
  - Applied `maxRedirects` guard (`config.followRedirects ? config.maxRedirects : null`) consistently to PATCH, DELETE, HEAD, download, downloadToFile, and uploadFile — these methods were previously passing `config.maxRedirects` unconditionally


## 2.2.1
  - Fixed `Dio` redirect option handling by only setting `maxRedirects` when `followRedirects` is enabled
  - Applied this fix consistently to base `Dio options` and per-request `Dio` options in `GET` and `POST` flows

## 2.2.0
- Added `validateStatus` parameter (`bool Function(int?)?`) to all HTTP methods (`get`, `getJson`, `getJsonList`, `post`, `postJson`, `put`, `putJson`, `patch`, `patchJson`, `delete`, `deleteJson`, `head`, `download`, `downloadToFile`, `uploadFile`) — allows per-request control over which status codes are treated as valid
- Fixed Dio request options: `followRedirects`, `maxRedirects`, and `validateStatus` are now correctly forwarded to all `_perform*` methods — previously only `receiveTimeout` and `headers` were passed through
- flutter/Dart SDKs updated

## 2.1.1
- `s_packages` package dependency upgraded

## 2.1.0
- `s_packages` dependency upgraded to ^1.3.0
- Added `putJson<T>()` typed variant for PUT requests with JSON deserialization
- Added `patchJson<T>()` typed variant for PATCH requests with JSON deserialization
- Added `deleteJson<T>()` typed variant for DELETE requests with JSON deserialization

## 2.0.0
- package no longer holds the source code for it, but exports/exposes the `s_packages` package instead, which will hold this package's latest source code.
- The only future changes to this package will be made via `s_packages` package dependency upgrades, in order to bring the new fixes or changes to this package
- dependent on `s_packages`: ^1.1.2



## 1.1.0

* dio package dependency upgraded from 5.4.0 to 5.9.0 --> FileAccessMode feature added to downloadToFile, now supporting directly writing to disk and supports resume capabilities.
* README updated


## 1.0.0

* Initial release
* **Safe by Default**: All requests wrapped in internal try-catch blocks
  - Never throws unhandled exceptions - guaranteed safe execution
  - All errors captured and returned as `ClientException` in tuple
  - No need for manual try-catch blocks around HTTP requests
* **Unified API**: All HTTP methods return result tuples AND support optional callbacks
  - Every method returns `(ClientResponse?, ClientException?)` tuple for structured error handling
  - Optional callbacks (`onSuccess`, `onError`, `onHttpError`, `onStatus`, `onProgress`) for reactive programming
  - Use tuple-only, callbacks-only, or BOTH approaches simultaneously
  - Callbacks and tuple results receive the same response object
* Dual backend support (http and dio packages)
  - Switch between lightweight `http` or feature-rich `dio` at runtime
  - Identical API regardless of backend
* Type-safe JSON parsing
  - `getJson<T>`, `getJsonList<T>`, `postJson<T>` with automatic deserialization
  - Custom `fromJson` functions for type safety
* Customizable success and error status codes
  - Define which HTTP status codes are success vs error
  - Per-request or global configuration
* Built-in interceptors:
  - `LoggingInterceptor`: Request/response logging with pretty-print JSON
  - `AuthInterceptor`: Bearer, API Key, Basic, Custom authentication
  - `CacheInterceptor`: In-memory response caching
  - `ClientInterceptor`: Base class for custom interceptors
* Automatic retry logic with exponential backoff
  - Configurable retry attempts, delays, and status codes
  - Per-request cancellation support
* File operations
  - Upload files with progress callbacks
  - Download files with progress callbacks
* Request cancellation
  - Cancel individual requests by key
  - Cancel all pending requests
* Comprehensive test coverage
* Clean, modern Dart 3+ API with records (tuples), sealed classes, and pattern matching
