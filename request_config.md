# Request Configuration

This guide provides a comprehensive overview of configuring Axios requests, including all available options and their usage. Understanding these configuration options will help you customize your HTTP requests to meet your specific needs.

## Table of Contents

1. [Basic Configuration](#basic-configuration)
2. [Request Method](#request-method)
3. [URL and Parameters](#url-and-parameters)
4. [Headers](#headers)
5. [Request Data](#request-data)
6. [Request Transformations](#request-transformations)
7. [Response Transformations](#response-transformations)
8. [Timeout](#timeout)
9. [XSRF Protection](#xsrf-protection)
10. [Content Length](#content-length)
11. [Validation](#validation)
12. [Advanced Options](#advanced-options)

## Basic Configuration

When making a request with Axios, you can provide a configuration object to customize various aspects of the request. Here's a basic example:

```javascript
axios.request({
  method: 'get',
  url: 'https://api.example.com/data',
  headers: {
    'Authorization': 'Bearer token123'
  },
  params: {
    id: 123
  }
});
```

## Request Method

The `method` property specifies the HTTP method to be used for the request. Supported methods include:

- `get`
- `post`
- `put`
- `patch`
- `delete`
- `head`
- `options`

Example:

```javascript
axios.request({
  method: 'post',
  url: 'https://api.example.com/data'
});
```

## URL and Parameters

The `url` property specifies the server URL for the request. You can also use the `baseURL` property to set a base URL for all requests:

```javascript
axios.request({
  baseURL: 'https://api.example.com',
  url: '/data'
});
```

Use the `params` property to send URL parameters:

```javascript
axios.request({
  url: 'https://api.example.com/data',
  params: {
    id: 123,
    page: 1
  }
});
```

## Headers

Set custom headers using the `headers` property:

```javascript
axios.request({
  url: 'https://api.example.com/data',
  headers: {
    'Authorization': 'Bearer token123',
    'Content-Type': 'application/json'
  }
});
```

## Request Data

Use the `data` property to send data in the request body:

```javascript
axios.request({
  method: 'post',
  url: 'https://api.example.com/data',
  data: {
    name: 'John Doe',
    email: 'john@example.com'
  }
});
```

## Request Transformations

The `transformRequest` option allows you to modify the request data before it is sent to the server:

```javascript
axios.request({
  // ... other options
  transformRequest: [function (data, headers) {
    // Perform the transformation
    return data;
  }]
});
```

## Response Transformations

Similarly, use `transformResponse` to modify the response data:

```javascript
axios.request({
  // ... other options
  transformResponse: [function (data) {
    // Perform the transformation
    return data;
  }]
});
```

## Timeout

Set a timeout for the request using the `timeout` property (in milliseconds):

```javascript
axios.request({
  url: 'https://api.example.com/data',
  timeout: 5000 // 5 seconds
});
```

## XSRF Protection

Configure XSRF protection using `xsrfCookieName` and `xsrfHeaderName`:

```javascript
axios.request({
  // ... other options
  xsrfCookieName: 'XSRF-TOKEN',
  xsrfHeaderName: 'X-XSRF-TOKEN'
});
```

## Content Length

Control the maximum content length using `maxContentLength` and `maxBodyLength`:

```javascript
axios.request({
  // ... other options
  maxContentLength: 2000,
  maxBodyLength: 2000
});
```

## Validation

Use the `validateStatus` function to determine if the response status is acceptable:

```javascript
axios.request({
  // ... other options
  validateStatus: function (status) {
    return status >= 200 && status < 300; // Default
  }
});
```

## Advanced Options

- `adapter`: Specify the adapter to use for the request (e.g., 'xhr', 'http', 'fetch').
- `allowAbsoluteUrls`: Allow absolute URLs in the request configuration.
- `transitional`: Configure transitional options for backwards compatibility.

For more advanced configurations, refer to the Axios source code and API documentation.