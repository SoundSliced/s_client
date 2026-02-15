 

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
