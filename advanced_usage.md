# Advanced Usage Guide

This guide covers advanced topics for using Axios, including request/response transformations, custom instance creation, and handling large file uploads/downloads.

## Table of Contents

1. [Custom Instance Creation](#custom-instance-creation)
2. [Request and Response Transformations](#request-and-response-transformations)
3. [Handling Large File Uploads](#handling-large-file-uploads)
4. [Handling Large File Downloads](#handling-large-file-downloads)

## Custom Instance Creation

Axios allows you to create custom instances with specific configurations. This is useful when you need to make multiple requests with the same base settings.

```javascript
import axios from 'axios';

const instance = axios.create({
  baseURL: 'https://api.example.com',
  timeout: 5000,
  headers: {'X-Custom-Header': 'foobar'}
});

// Use the custom instance
instance.get('/endpoint')
  .then(response => {
    // Handle response
  })
  .catch(error => {
    // Handle error
  });
```

## Request and Response Transformations

Axios provides powerful request and response transformation capabilities, allowing you to modify data before it's sent or after it's received.

### Request Transformations

```javascript
const instance = axios.create({
  transformRequest: [function (data, headers) {
    // Transform the data for the request
    return JSON.stringify(data);
  }],
});
```

### Response Transformations

```javascript
const instance = axios.create({
  transformResponse: [function (data) {
    // Transform the response data
    return JSON.parse(data);
  }],
});
```

## Handling Large File Uploads

For large file uploads, you can use the `FormData` API along with Axios:

```javascript
import axios from 'axios';

const uploadFile = async (file) => {
  const formData = new FormData();
  formData.append('file', file);

  try {
    const response = await axios.post('/upload', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      },
      onUploadProgress: (progressEvent) => {
        const percentCompleted = Math.round((progressEvent.loaded * 100) / progressEvent.total);
        console.log(`Upload progress: ${percentCompleted}%`);
      }
    });
    return response.data;
  } catch (error) {
    console.error('Error uploading file:', error);
    throw error;
  }
};
```

## Handling Large File Downloads

For large file downloads, you can use the `responseType` option to receive the data as a stream:

```javascript
import axios from 'axios';
import fs from 'fs';

const downloadFile = async (url, outputPath) => {
  const writer = fs.createWriteStream(outputPath);

  try {
    const response = await axios({
      method: 'get',
      url: url,
      responseType: 'stream'
    });

    response.data.pipe(writer);

    return new Promise((resolve, reject) => {
      writer.on('finish', resolve);
      writer.on('error', reject);
    });
  } catch (error) {
    console.error('Error downloading file:', error);
    throw error;
  }
};
```

This advanced usage guide covers some of the more complex features of Axios. By leveraging these capabilities, you can create more efficient and powerful HTTP requests in your applications.