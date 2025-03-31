# Axios Interceptors Tutorial

Axios interceptors are a powerful feature that allows you to intercept and modify HTTP requests and responses before they are handled by `then` or `catch`. This tutorial will guide you through using Axios interceptors effectively, with practical examples for common use cases.

## Table of Contents

1. [Introduction to Interceptors](#introduction-to-interceptors)
2. [Request Interceptors](#request-interceptors)
3. [Response Interceptors](#response-interceptors)
4. [Common Use Cases](#common-use-cases)
5. [Error Handling](#error-handling)
6. [Removing Interceptors](#removing-interceptors)

## Introduction to Interceptors

Axios provides two types of interceptors:

1. Request interceptors: These intercept the request before it's sent.
2. Response interceptors: These intercept the response before it reaches the `then` or `catch` block.

Interceptors are managed by the `InterceptorManager` class, which allows you to add, remove, and clear interceptors.

## Request Interceptors

Request interceptors are useful for modifying requests before they are sent. Here's how to add a request interceptor:

```javascript
axios.interceptors.request.use(
  function (config) {
    // Modify the request config
    return config;
  },
  function (error) {
    // Handle request errors
    return Promise.reject(error);
  }
);
```

Example: Adding an authentication token to all requests

```javascript
axios.interceptors.request.use(function (config) {
  const token = localStorage.getItem('authToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

## Response Interceptors

Response interceptors allow you to modify or handle responses before they reach the `then` or `catch` block. Here's how to add a response interceptor:

```javascript
axios.interceptors.response.use(
  function (response) {
    // Any status code within the range of 2xx triggers this function
    return response;
  },
  function (error) {
    // Any status codes outside the range of 2xx trigger this function
    return Promise.reject(error);
  }
);
```

Example: Handling refresh tokens

```javascript
axios.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    if (error.response.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      const refreshToken = localStorage.getItem('refreshToken');
      try {
        const { data } = await axios.post('/refresh-token', { refreshToken });
        localStorage.setItem('authToken', data.authToken);
        axios.defaults.headers.common['Authorization'] = `Bearer ${data.authToken}`;
        return axios(originalRequest);
      } catch (refreshError) {
        // Handle refresh token error (e.g., logout user)
        return Promise.reject(refreshError);
      }
    }
    return Promise.reject(error);
  }
);
```

## Common Use Cases

### 1. Logging

You can use interceptors to log requests and responses for debugging purposes:

```javascript
axios.interceptors.request.use((config) => {
  console.log('Request sent:', config);
  return config;
});

axios.interceptors.response.use((response) => {
  console.log('Response received:', response);
  return response;
});
```

### 2. Loading Indicators

Interceptors can be used to show and hide loading indicators:

```javascript
let requestsCounter = 0;

axios.interceptors.request.use((config) => {
  if (++requestsCounter === 1) {
    showLoadingIndicator();
  }
  return config;
});

axios.interceptors.response.use(
  (response) => {
    if (--requestsCounter === 0) {
      hideLoadingIndicator();
    }
    return response;
  },
  (error) => {
    if (--requestsCounter === 0) {
      hideLoadingIndicator();
    }
    return Promise.reject(error);
  }
);
```

### 3. Request Retrying

You can implement request retrying logic using interceptors:

```javascript
axios.interceptors.response.use(null, (error) => {
  if (error.config && error.response && error.response.status === 500) {
    return new Promise((resolve) => {
      setTimeout(() => resolve(axios(error.config)), 1000);
    });
  }
  return Promise.reject(error);
});
```

## Error Handling

Interceptors are an excellent place to handle common errors across your application:

```javascript
axios.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response) {
      switch (error.response.status) {
        case 400:
          console.error('Bad Request');
          break;
        case 401:
          console.error('Unauthorized');
          // Redirect to login page or refresh token
          break;
        case 404:
          console.error('Not Found');
          break;
        case 500:
          console.error('Internal Server Error');
          break;
        default:
          console.error('An error occurred');
      }
    } else if (error.request) {
      console.error('No response received');
    } else {
      console.error('Error setting up request', error.message);
    }
    return Promise.reject(error);
  }
);
```

## Removing Interceptors

If you need to remove an interceptor, you can use the `eject` method:

```javascript
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

To remove all interceptors, you can use the `clear` method:

```javascript
axios.interceptors.request.clear();
axios.interceptors.response.clear();
```

By utilizing Axios interceptors, you can create powerful and flexible HTTP clients that can handle a wide range of scenarios, from authentication to error handling and beyond. Experiment with different interceptor configurations to find the best setup for your application's needs.