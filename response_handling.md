# Response Handling in Axios

This guide explains how to handle responses in Axios, including accessing response data, headers, and status. We'll also provide examples of common response handling patterns.

## Table of Contents

1. [Response Object Structure](#response-object-structure)
2. [Accessing Response Data](#accessing-response-data)
3. [Handling Different Status Codes](#handling-different-status-codes)
4. [Working with Response Headers](#working-with-response-headers)
5. [Error Handling](#error-handling)
6. [Common Response Handling Patterns](#common-response-handling-patterns)

## Response Object Structure

When you make a request with Axios, the response object contains several properties:

- `data`: The response body
- `status`: The HTTP status code
- `statusText`: The HTTP status message
- `headers`: The response headers
- `config`: The original request configuration
- `request`: The request object

## Accessing Response Data

To access the response data, you can use the `data` property of the response object:

```javascript
axios.get('https://api.example.com/users')
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

## Handling Different Status Codes

Axios automatically rejects the promise for status codes outside the 2xx range. However, you can customize this behavior using the `validateStatus` configuration option:

```javascript
axios.get('https://api.example.com/users', {
  validateStatus: function (status) {
    return status < 500; // Resolve only if the status code is less than 500
  }
})
.then(response => {
  console.log('Status:', response.status);
})
.catch(error => {
  console.error('Error:', error);
});
```

## Working with Response Headers

You can access response headers using the `headers` property:

```javascript
axios.get('https://api.example.com/users')
  .then(response => {
    console.log('Content-Type:', response.headers['content-type']);
    console.log('Date:', response.headers.date);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

## Error Handling

When an error occurs, Axios provides an `AxiosError` object with additional information:

```javascript
axios.get('https://api.example.com/users')
  .then(response => {
    console.log('Data:', response.data);
  })
  .catch(error => {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls out of the range of 2xx
      console.error('Error status:', error.response.status);
      console.error('Error data:', error.response.data);
    } else if (error.request) {
      // The request was made but no response was received
      console.error('No response received:', error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.error('Error message:', error.message);
    }
  });
```

## Common Response Handling Patterns

### Checking for Successful Responses

```javascript
axios.get('https://api.example.com/users')
  .then(response => {
    if (response.status === 200) {
      console.log('Success:', response.data);
    } else {
      console.warn('Unexpected status code:', response.status);
    }
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### Handling Pagination

```javascript
function fetchAllUsers() {
  let allUsers = [];
  let page = 1;

  function fetchPage() {
    return axios.get(`https://api.example.com/users?page=${page}`)
      .then(response => {
        allUsers = allUsers.concat(response.data.users);
        if (response.data.hasNextPage) {
          page++;
          return fetchPage();
        }
        return allUsers;
      });
  }

  return fetchPage();
}

fetchAllUsers()
  .then(users => {
    console.log('All users:', users);
  })
  .catch(error => {
    console.error('Error fetching users:', error);
  });
```

### Transforming Response Data

```javascript
axios.get('https://api.example.com/users')
  .then(response => {
    const transformedData = response.data.map(user => ({
      id: user.id,
      fullName: `${user.firstName} ${user.lastName}`,
      email: user.email.toLowerCase()
    }));
    console.log('Transformed data:', transformedData);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

By following these patterns and understanding how to handle responses in Axios, you can effectively process and manage data from your API requests.