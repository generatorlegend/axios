# Migration Guide

This guide provides information on migrating from older versions of Axios to newer versions, highlighting breaking changes and new features.

## Migrating from 0.x to 1.x

### Breaking Changes

1. **AxiosError Changes**
   - The `config` property of `AxiosError` is now optional in the type definition.
   - `AxiosError` now includes a stack trace.

2. **Headers Handling**
   - The `AxiosHeaders` class has been introduced for better header management.
   - Common header merging behavior has changed.

3. **Request Config**
   - The `params` serialization has changed. A new `paramsSerializer` config has been added.
   - `withCredentials` behavior has changed for CSRF protection. Use the new `withXSRFToken` option along with `withCredentials` to get the old behavior.

4. **Response Handling**
   - The `transformResponse` function now receives the HTTP status code as an additional parameter.

5. **Adapter Changes**
   - The adapter loading logic has been improved for clearer error messages.

### New Features

1. **Enhanced FormData Handling**
   - Added support for spec-compliant FormData and Blob types.
   - Introduced `formSerializer` config option for customizing FormData serialization.

2. **Improved TypeScript Support**
   - Added `AxiosHeaderValue` type export.
   - Improved types for various Axios interfaces and configurations.

3. **New Utility Functions**
   - Added `formToJSON` method for converting FormData to JSON.
   - Introduced `toFormData` helper with additional options.

4. **Cancellation Enhancements**
   - Added support for AbortController for request cancellation.

5. **URL Handling**
   - Added support for URL objects in request config.

6. **Custom Serializers**
   - Introduced `paramsSerializer` config for custom parameter serialization.

### Upgrade Steps

1. Update your Axios dependency to version 1.x:
   ```
   npm install axios@latest
   ```

2. Review your error handling code to account for the changes in `AxiosError`.

3. Update header management to use the new `AxiosHeaders` class where applicable.

4. Review and update your request configurations, especially for params serialization and CSRF handling.

5. If you're using TypeScript, update your types and take advantage of the new type exports.

6. Test your application thoroughly after the upgrade, paying special attention to request/response handling and error management.

## Migrating from 1.x to Latest

For migrations between 1.x versions, please refer to the [Changelog](https://github.com/axios/axios/blob/master/CHANGELOG.md) for detailed information on changes and new features in each release.

Remember to always test your application thoroughly after upgrading to ensure compatibility and to take advantage of new features and improvements.