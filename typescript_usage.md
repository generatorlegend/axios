# Using Axios with TypeScript

Axios provides excellent TypeScript support out of the box. This guide will help you understand how to use Axios with TypeScript, including type definitions for requests, responses, and configuration options.

## Installation

First, make sure you have Axios installed in your project:

```bash
npm install axios
```

TypeScript type definitions are included in the package, so you don't need to install them separately.

## Basic Usage

To use Axios with TypeScript, you can import it as follows:

```typescript
import axios from 'axios';
```

## Making Requests

When making requests, you can specify the expected response type using generics:

```typescript
interface User {
  id: number;
  name: string;
}

axios.get<User>('/user/12345')
  .then(response => {
    const user: User = response.data;
    console.log(user.name);
  });
```

## Request Configuration

Axios provides type definitions for request configuration. You can use the `AxiosRequestConfig` interface to type your configuration object:

```typescript
import { AxiosRequestConfig } from 'axios';

const config: AxiosRequestConfig = {
  url: '/user',
  method: 'post',
  baseURL: 'https://api.example.com',
  headers: {
    'Content-Type': 'application/json'
  },
  data: {
    name: 'John Doe'
  }
};

axios(config);
```

## Response Handling

The `AxiosResponse` interface provides type information for the response:

```typescript
import { AxiosResponse } from 'axios';

interface User {
  id: number;
  name: string;
}

axios.get<User>('/user/12345')
  .then((response: AxiosResponse<User>) => {
    console.log(response.data.name);
    console.log(response.status);
    console.log(response.headers['content-type']);
  });
```

## Error Handling

Axios provides the `AxiosError` type for error handling:

```typescript
import { AxiosError } from 'axios';

axios.get('/user/12345')
  .catch((error: AxiosError) => {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls out of the range of 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // The request was made but no response was received
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
  });
```

## Interceptors

You can use TypeScript with Axios interceptors for both requests and responses:

```typescript
import { InternalAxiosRequestConfig, AxiosResponse } from 'axios';

// Request interceptor
axios.interceptors.request.use(
  (config: InternalAxiosRequestConfig) => {
    // Modify config here
    return config;
  },
  (error: any) => {
    return Promise.reject(error);
  }
);

// Response interceptor
axios.interceptors.response.use(
  (response: AxiosResponse) => {
    // Modify response here
    return response;
  },
  (error: any) => {
    return Promise.reject(error);
  }
);
```

## Custom Instance

When creating a custom Axios instance, you can specify default configuration options:

```typescript
import axios, { AxiosInstance, CreateAxiosDefaults } from 'axios';

const config: CreateAxiosDefaults = {
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: {'X-Custom-Header': 'foobar'}
};

const instance: AxiosInstance = axios.create(config);
```

## Advanced Types

Axios provides several advanced types that you can use in your TypeScript code:

- `Method`: Represents HTTP methods (e.g., 'get', 'post', 'put', etc.)
- `AxiosPromise`: A Promise that resolves with an AxiosResponse
- `CancelToken`: Used for request cancellation
- `AxiosProxyConfig`: For configuring proxy settings

Example usage:

```typescript
import { Method, AxiosPromise, CancelToken, AxiosProxyConfig } from 'axios';

const method: Method = 'get';

const promise: AxiosPromise<User> = axios.get('/user/12345');

const source = CancelToken.source();
axios.get('/user/12345', {
  cancelToken: source.token
});
source.cancel('Operation canceled by the user.');

const proxyConfig: AxiosProxyConfig = {
  host: '127.0.0.1',
  port: 9000
};
```

By leveraging these TypeScript features and type definitions provided by Axios, you can write more robust and type-safe code when working with HTTP requests in your TypeScript projects.