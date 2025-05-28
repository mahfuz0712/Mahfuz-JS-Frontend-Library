# Mahfuz-JS Frontend Library Documentation

Mahfuz-JS is a lightweight and extensible frontend library designed to simplify API interactions and version management in modern JavaScript applications. This library provides a structured interface to interact with REST APIs and manage application update logic using GitHub releases.

---

## Table of Contents

1. [Installation](#installation)
2. [Frontend Class](#frontend-class)

   * [ConnectToBackend](#connecttobackend)
   * [AddInterceptors](#addinterceptors)
   * [HTTP Methods (GET, POST, PUT, PATCH, DELETE)](#http-methods)
   * [Concurrent Requests](#concurrent-requests)
   * [Cancelable Requests](#cancelable-requests)
   * [File Upload](#file-upload)
3. [UpdateChecker Class](#updatechecker-class)

   * [CheckLatestVersion](#checklatestversion)
4. [Error Handling](#error-handling)

---

## Installation

Install via npm:

```bash
npm install mahfuz-js-frontend
```

Import into your project:

```javascript
import { Frontend, UpdateChecker } from "mahfuz-js";
```

---

## Frontend Class

### Initialization

```javascript
const frontend = new Frontend();
```

### ConnectToBackend

Establish a connection to your backend API.

```javascript
frontend.ConnectToBackend(import.meta.env.VITE_BACKEND_URL);
```

If no backend URL is provided, a SweetAlert modal will prompt the user with setup instructions.

### AddInterceptors

Log all requests and responses for easier debugging.

```javascript
frontend.AddInterceptors();
```

### HTTP Methods

All HTTP methods return the full response object or the error.

```javascript
await frontend.get("/api/users");
await frontend.post("/api/create", { name: "John" });
await frontend.put("/api/user/1", { name: "Updated" });
await frontend.patch("/api/user/1", { age: 30 });
await frontend.delete("/api/user/1");
```

### Concurrent Requests

Execute multiple requests simultaneously.

```javascript
const responses = await frontend.ConcurrentRequests([
  frontend.get("/api/a"),
  frontend.get("/api/b")
]);
```

### Cancelable Requests

Create and cancel long-running requests.

```javascript
const source = frontend.CancelRequest();
frontend.Server.post("/api/data", {}, { cancelToken: source.token });
source.cancel("Operation canceled by user.");
```

Alternative using helper:

```javascript
await frontend.SendWithCancel("/api/data", { id: 123 });
```

### File Upload

Upload files with progress tracking.

```javascript
const response = await frontend.UploadFileToBackend("/api/upload", file);
```

---

## UpdateChecker Class

### Initialization

```javascript
const checker = new UpdateChecker({
  GithubUsername: "your-username",
  RepoName: "your-repo",
  RepoToken: "your-github-token"
});
```

### CheckLatestVersion

Fetch the latest release based on OS.

```javascript
const result = await checker.CheckLatestVersion("windows");
if (result.success) {
  window.open(result.downloadUrl);
}
```

Supported OS values:

* `windows`
* `mac`
* `linux`

---

## Error Handling

All methods return either a response object or an error. Use `try-catch` or conditional checks to handle them properly.

```javascript
try {
  const response = await frontend.get("/api/data");
  if (response.status === 200) {
    // handle success
  }
} catch (error) {
  console.error("API Error:", error);
}
```

---

## Notes

* Always ensure your environment variable `VITE_BACKEND_URL` is set.
* Protect your GitHub token. Avoid exposing it in the frontend in production.

---

Mahfuz-JS: Built for simplicity, designed for power.
