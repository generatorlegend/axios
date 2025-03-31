# Getting Started with Axios

Axios is a popular, promise-based HTTP client for making requests from both the browser and Node.js. This guide will help you get started with Axios by covering installation and basic usage.

## Installation

You can install Axios using npm, yarn, pnpm, or bun. Choose the package manager that fits your project setup:

```bash
# Using npm
npm install axios

# Using yarn
yarn add axios

# Using pnpm
pnpm add axios

# Using bun
bun add axios
```

## Making Requests

Once installed, you can start making HTTP requests with Axios. Here are examples of common HTTP methods:

### GET Request

To fetch data from a server:

```javascript
import axios from 'axios';

axios.get('https://api.example.com/users')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### POST Request

To send data to a server:

```javascript
axios.post('https://api.example.com/users', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### PUT Request

To update existing data on a server:

```javascript
axios.put('https://api.example.com/users/1', {
  firstName: 'Barney',
  lastName: 'Rubble'
})
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### DELETE Request

To remove data from a server:

```javascript
axios.delete('https://api.example.com/users/1')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

## Using Async/Await

Axios works great with async/await for more readable asynchronous code:

```javascript
async function getUser() {
  try {
    const response = await axios.get('https://api.example.com/users/1');
    console.log(response.data);
  } catch (error) {
    console.error('Error:', error);
  }
}

getUser();
```

## Configuration

You can create a custom instance of Axios with specific settings:

```javascript
const instance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

// Now you can use this instance for requests
instance.get('/users')
  .then(response => {
    console.log(response.data);
  });
```

This is just the beginning of what you can do with Axios. For more advanced usage, including request and response interceptors, cancellation, and more, check out the full documentation.