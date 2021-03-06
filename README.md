[![Pub Version](https://img.shields.io/pub/v/msal_js)](https://pub.dev/packages/msal_js)

# MSAL.js for Dart
A Dart wrapper for the [Microsoft Authentication Library for JavaScript (MSAL.js)](https://github.com/AzureAD/microsoft-authentication-library-for-js). 

## Contents
- [Install msal.js](#install-msaljs)
- [Usage](#usage)
- [Differences from msal.js](#differences-from-msaljs)
- [msal.js Support Table](#msaljs-support-table)

## Install msal.js
This package does not come with msal.js. Please refer to the [msal.js documentation](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/MSAL-Installation) for installation.

See the [msal.js support table](#msaljs-support-table) below to determine which version of msal.js should be used with each version of this package.

## Usage
For more complete usage information, see this [example](./example/example.md). 

For usage in a Flutter Web app, see the [Flutter Web example](./example/flutter_web_example.md). Please also see the [known issues and their workarounds with Flutter](./example/flutter_web_example.md#known-issues).

Also see the [msal.js documentation](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki). Class, function, and parameter names are *almost* all the same between this wrapper and msal.js (please see ["Differences from msal.js"](#differences-from-msaljs) for more information). Following msal.js examples should be mostly straight-forward.

Short getting started example:
```dart
import 'package:msal_js/msal_js.dart';

void main() {
  // Important note about the below 'options' objects:
  //
  // To set an option to `undefined`, simply don't call
  // the setter for that option at all. This will let
  // msal.js use the option's default value. Setting an
  // option to `null` will not use its default value.

  // Create an MSAL logger (optional)
  var logger = Logger(
    _loggerCallback,
    LoggerOptions()..level = LogLevel.verbose, // log everything
  );

  // Configure and create an MSAL authentication context
  // Note: Only clientId is required
  var config = Configuration()
    ..auth = (AuthOptions()..clientId = 'your_client_id');

  var userAgentApplication = UserAgentApplication(config);

  // If you plan on using the redirect flow, register a callback
  userAgentApplication.handleRedirectCallback(_authCallback);

  // Login by calling either:
  // - userAgentApplication.loginRedirect
  // - userAgentApplication.loginPopup

  // After login, acquire an access token by calling:
  // userAgentApplication.acquireTokenSilent

  // If acquiring the token silently fails, use either:
  // - userAgentApplication.acquireTokenRedirect
  // - userAgentApplication.acquireTokenPopup

  // See the msal.js documentation for more information.
}

void _loggerCallback(LogLevel level, String message, bool containsPii) {
  print('[$level] $message');
}

void _authCallback(AuthException error, [AuthResponse response]) {
  // ...
}
```

## Differences from msal.js
This package has a few minor differences from the JavaScript and TypeScript APIs in msal.js. These are mainly due to incompatibilities between TypeScript and Dart and quality-of-life differences to provide a more idiomatic Dart API.

- APIs returning JavaScript `Promise`s instead return Dart `Future`s.
- TypeScript string unions (such as `cacheLocation`) are represented as Dart enums.
- MSAL errors are represented as Dart exceptions and use the suffix `Exception` instead of `Error` (e.g. `AuthError` in msal.js is `AuthException` in this wrapper).
- Typescript interfaces are represented as a full Dart type. Instead of passing a map which meets the interface requirements, an actual type must be constructed (e.g. instead of `loginPopup({scopes: []})` you would do `loginPopup(new AuthRequest()..scopes = [])`).

## msal.js Support Table
> **Important Note**: The msal.js library *does not* follow semantic versioning like Dart packages do. This package makes a best effort to keep its version inline with msal.js without breaking semantic versioning.

The following table showcases the msal.js version that is targeted by each version of this package.

Newer versions of msal.js will likely work on older versions of this package. For example, msal.js 1.3.0 will work on 1.2.0 of this package, but the new APIs will be unavailable. Avoid using msal.js versions older than the package version that targets it. APIs may be available in Dart that do not exist in the older JS version.

Often, msal.js will make API additions and changes in their "patch" releases. These changes won't be reflected in this package until the next "minor" release to avoid breaking semantic versioning.

| Dart (msal_js)     | JavaScript (msal.js) |
| ------------------ | -------------------- |
| **1.3.0**          | 1.3.2                |
| **1.2.0**          | 1.2.0                |
| **1.1.0**          | 1.1.0                |
| **1.0.0**          | 1.0.0                |
| **0.3.2**          | 0.2.4                |
| **0.2.0**          | 0.2.3                |
| **0.1.0**          | 0.1.5                |
