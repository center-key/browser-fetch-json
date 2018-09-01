# browser-fetch-json
<img src=https://raw.githubusercontent.com/center-key/browser-fetch-json/master/logos.png
   align=right width=200 alt=logos>

_A thin wrapper around the browser's native Fetch API just for JSON_

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/center-key/browser-fetch-json/blob/master/LICENSE.txt)
&nbsp;
[![npm](https://img.shields.io/npm/v/browser-fetch-json.svg)](https://www.npmjs.com/package/browser-fetch-json)
&nbsp;
[![Known Vulnerabilities](https://snyk.io/test/github/center-key/browser-fetch-json/badge.svg)](https://snyk.io/test/github/center-key/browser-fetch-json)
&nbsp;
[![Build Status](https://travis-ci.org/center-key/browser-fetch-json.svg)](https://travis-ci.org/center-key/browser-fetch-json)

Why would you fetch anything but json? ;)

### 1) Setup
In a web page:
```html
<script src=fetch-json.min.js></script>
```

From the [jsdelivr.com CDN](https://www.jsdelivr.com/package/npm/browser-fetch-json):
```html
<script src=https://cdn.jsdelivr.net/npm/browser-fetch-json@0.2/fetch-json.min.js></script>
```

Or install as a module:
```shell
$ npm install browser-fetch-json
```
Then import with:
```javascript
const fetchJson = require('browser-fetch-json');
```

### 2) Examples
#### HTTP GET
Fetch the NASA Astronomy Picture of the Day:
```javascript
// NASA APOD
const url =    'https://api.nasa.gov/planetary/apod';
const params = { api_key: 'DEMO_KEY' };
function handleData(data) {
   console.log('The NASA APOD for today is at: ' + data.url);
   }
fetchJson.get(url, params).then(handleData);
```
#### HTTP POST
Create a resource for the planet Jupiter:
```javascript
// Create Jupiter
const resource = { name: 'Jupiter', position: 5 };
function handleData(data) {
   console.log(data);  //HTTP response body as an object literal
   }
fetchJson.post('https://httpbin.org/post', resource)
   .then(handleData)
   .catch(console.error);
```

### 3) Leverages the Fetch API
**browser-fetch-json** uses the browser's native **[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)**.

For comparison, the above POST example to create a planet would be done directly using the **Fetch API** with the code:
```javascript
// Create Jupiter (with Fetch API instead of browser-fetch-json)
const resource = { name: 'Jupiter', position: 5 };
const options = {
   method: 'POST',
   headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
      },
   body: JSON.stringify(resource)
   };
function handleData(data) {
   console.log(data);  //HTTP response body as an object literal
   }
fetch('https://httpbin.org/post', options)
   .then(response => response.json())
   .then(handleData)
   .catch(console.error);
```
The examples for **browser-fetch-json** and the **Fetch API** each produce the same output.

### 4) Details
The **browser-fetch-json** module automatically:
1. Serializes the body payload with `JSON.stringify()`.
1. Adds the JSON data type (`'application/json'`) to the HTTP headers.
1. Builds the URL query string from the `params` object for GET requests.
1. Runs `.json()` on the response from the promise.
1. Sets `credentials` to `'same-origin'` to support user sessions for frameworks/servers such as Grails, Rails, PHP, Flask, etc.

### 5) API
The format for using **browser-fetch-json** is:
#### GET
```javascript
fetchJson.get(url, params, options).then(callback);
```
#### POST
```javascript
fetchJson.post(url, resource, options).then(callback);
```
#### PUT
```javascript
fetchJson.put(url, resource, options).then(callback);
```
#### PATCH
```javascript
fetchJson.patch(url, resource, options).then(callback);
```
#### DELETE
```javascript
fetchJson.delete(url, resource, options).then(callback);
```
Notes:
1. Only the `url` parameter is required.&nbsp; The other parameters are optional.
1. The `params` object for `fetchJson.get()` is converted into a query string and appended to the `url`.
1. The `resource` object is turned into the body of the HTTP request.
1. The `options` parameter is passed through to the **Fetch API** (see the MDN **Fetch API** documentation for supported **[`init` options](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters)**).

#### Dynamic HTTP method
If you need to programmatically set the method, use the format:
```javascript
fetchJson.request(method, url, data, options).then(callback);
```
Where `method` is `'GET'`, `'POST'`, `'PUT'`, `'PATCH'`, or `'DELETE'`, and `data` represents
either `params` or `resource`.

#### Logging
Enable basic logging to the console with:
```javascript
fetchJson.enableLogger();
```
Pass in a function to use a custom logger or pass in `false` to disable logging.

### 6) Legacy web browsers
To support really old browsers, include polyfills for
[Promise](https://github.com/taylorhakes/promise-polyfill/) and
[Fetch API](https://github.com/github/fetch):
```html
<script src=https://cdn.jsdelivr.net/npm/promise-polyfill@8.1/dist/polyfill.min.js></script>
<script src=https://cdn.jsdelivr.net/npm/whatwg-fetch@2.0/fetch.min.js></script>
```

### 7) Related
[node-fetch-json](https://github.com/center-key/node-fetch-json) - A version for node

### 8) Questions or enhancements
Feel free to submit an [issue](https://github.com/center-key/browser-fetch-json/issues).

---
[MIT License](LICENSE.txt)