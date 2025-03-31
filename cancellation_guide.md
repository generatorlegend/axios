# Cancellation Guide

This guide explains how to cancel requests in Axios using both the `CancelToken` API and the newer `AbortController` API.

## Using CancelToken

The `CancelToken` is a class provided by Axios that allows you to cancel requests. Here's how to use it:

1. Create a cancel token source:

```javascript
const CancelToken = axios.CancelToken;
const source = CancelToken.source();
```

2. Pass the cancel token to the request config:

```javascript
axios.get('/api/data', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled:', thrown.message);
  } else {
    // handle error
  }
});
```

3. Cancel the request:

```javascript
source.cancel('Operation canceled by the user.');
```

You can also create a cancel token using a function:

```javascript
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/api/data', {
  cancelToken: new CancelToken(function executor(c) {
    cancel = c;
  })
});

// Cancel the request
cancel('Operation canceled by the user.');
```

## Using AbortController

Axios also supports the `AbortController` API, which is a more modern approach to cancellation:

1. Create an AbortController:

```javascript
const controller = new AbortController();
```

2. Pass the signal to the request config:

```javascript
axios.get('/api/data', {
  signal: controller.signal
}).catch(function (error) {
  if (error.name === 'AbortError') {
    console.log('Request canceled:', error.message);
  } else {
    // handle error
  }
});
```

3. Abort the request:

```javascript
controller.abort();
```

## Converting CancelToken to AbortSignal

If you're working with a library that expects an `AbortSignal`, you can convert a `CancelToken` to an `AbortSignal`:

```javascript
const source = CancelToken.source();
const signal = source.token.toAbortSignal();

// Use the signal with other APIs
fetch('/api/data', { signal });
```

## Best Practices

- Use `AbortController` for new projects, as it's a more standardized API.
- Handle cancellation errors appropriately in your catch blocks.
- Clean up cancel tokens or abort controllers when components unmount in frameworks like React.

Remember that cancellation is useful for scenarios like navigating away from a page before a request completes or implementing a timeout for long-running requests.