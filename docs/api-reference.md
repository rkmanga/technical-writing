---
<section class="section" markdown="1">

<h2 class="section-title"><span class="icon-holder"><i class="fas fa-code"></i></span> API Reference / Developer Guide</h2>

<div class="annotation-block" style="background: #11-0602; border: 0px solid #c3ddd5; border-radius: 4px; padding: 18px 22px; margin-bottom: 32px;">
  <h4 style="margin-top: 0; color: #02ffff; font-size: .75rem;">About This Sample</h4>
  <table style="width: 100%; font-size: 0.88rem; border-collapse: collapse;">
    <tr>
      <td style="width: 130px; font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Product</td>
      <td style="padding: 4px 0; color: #11-0602;">Nexus Task API — a RESTful task management API for the Nexus project management platform, enabling developers to programmatically create, retrieve, update, and delete tasks.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Audience</td>
      <td style="padding: 4px 0; color: ##11-0602;">Backend developers integrating task management capabilities into internal tools, dashboards, or custom applications, with working knowledge of REST APIs, HTTP, and JWT authentication.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Tools Used</td>
      <td style="padding: 4px 0; color: ##11-0602;">OpenAPI 3.0, Markdown, Postman (for validating request/response examples and JWT auth flows), VS Code, Git.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Challenge Solved</td>
      <td style="padding: 4px 0; color: ##11-0602;">The Nexus Task API exposes a focused set of CRUD operations with several interdependent concerns — JWT authentication (including token generation and refresh), cursor-based pagination, rate limiting, and a shared error schema. The challenge was consolidating all of this into a single, scannable reference covering every endpoint, its parameters, and its error cases, so developers could complete an integration without switching between multiple documentation pages.</td>
    </tr>
  </table>
</div>

---

The Nexus Task API documentation provides developers with the methods necessary to programmatically interact with task resources.

## Overview
The Nexus Task API is a RESTful interface designed for developers to programmatically create, manage, and retrieve tasks within the Nexus project management platform. It allows seamless integration of task management capabilities into internal tools, dashboards, and custom applications.

**Base URL:**
```text
https://api.nexus.com/v1
```

**Versioning:**
The API uses URI-based versioning. The current stable version is `v1`. Backward-incompatible changes will result in a new major version number (e.g., `v2`). 

**Data Formats:**
The API accepts and returns JSON exclusively. Clients must set the `Content-Type: application/json` header for all requests containing a body, and `Accept: application/json` for all requests.

## Authentication
The Nexus Task API uses JSON Web Tokens (JWT) via Bearer Auth to authenticate requests.

**Obtaining a Token:**
Generate a token by sending your API Key and Secret to the `/auth/token` endpoint. Tokens are valid for 60 minutes.

**Passing the Token:**
Include the token in the `Authorization` HTTP header of every request.

**Refreshing a Token:**
Before the 60-minute expiration, call the `/auth/refresh` endpoint using your active token to receive a new one without requiring your API Key and Secret.

**Example Authorization Header:**
```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
     https://api.nexus.com/v1/tasks
```

## Endpoints

### GET /tasks
Retrieves a paginated list of tasks associated with the authenticated user's workspace. Use this endpoint to display task directories or sync task data to local databases.

**Request Parameters:**

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `limit` | integer | Optional | The maximum number of tasks to return per page. Default is 20, max is 100. |
| `cursor` | string | Optional | A pagination token used to fetch the next set of results. |
| `status` | string | Optional | Filters the returned tasks by status (e.g., `pending`, `in_progress`, `completed`). |

**Request Body:**
*No request body required.*

**Response Example (200 OK):**
```json
{
  "data": [
    {
      "id": "tsk_01h8a9vbc",
      "title": "Update database schema",
      "description": "Add the new user preference columns to the main PostgreSQL database.",
      "status": "in_progress",
      "assignee_id": "usr_99x817z",
      "due_date": "2026-04-15T17:00:00Z",
      "created_at": "2026-03-10T09:15:30Z",
      "updated_at": "2026-03-19T14:22:10Z"
    }
  ],
  "pagination": {
    "next_cursor": "cXVlcnljdXJzb3I=",
    "has_more": true
  }
}
```

**Error Responses:**

-   **400 Bad Request:** Occurs if `limit` exceeds 100 or `status` contains an invalid value. Verify query parameter formatting.
-   **401 Unauthorized:** Occurs if the JWT is missing, invalid, or expired. Obtain a new token.
*   **429 Too Many Requests:** Occurs if the API rate limit is exceeded. Implement exponential backoff.

**Error Example (400 Bad Request):**
```json
{
  "error": {
    "code": "invalid_parameter",
    "message": "The 'limit' parameter must be an integer between 1 and 100.",
    "target": "limit"
  }
}
```

### POST /tasks
Creates a new task in the workspace. Use this endpoint when generating tasks from external triggers or user input in custom applications.

**Request Parameters:**

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `title` | string | Required | The headline or name of the task. Max 100 characters. |
| `description` | string | Optional | Detailed explanation of the task requirements. |
| `status` | string | Optional | The initial state. Must be `pending`, `in_progress`, or `completed`. Defaults to `pending`. |
| `assignee_id` | string | Optional | The unique identifier of the user responsible for the task. |
| `due_date` | string | Optional | The deadline for the task, formatted as an ISO 8601 datetime string. |

**Request Body Example:**
```json
{
  "title": "Provision staging servers",
  "description": "Deploy 3 new AWS EC2 instances for the Q3 release staging environment.",
  "status": "pending",
  "assignee_id": "usr_99x817z",
  "due_date": "2026-04-01T12:00:00Z"
}
```

**Response Example (201 Created):**
```json
{
  "id": "tsk_01h8a9xdf",
  "title": "Provision staging servers",
  "description": "Deploy 3 new AWS EC2 instances for the Q3 release staging environment.",
  "status": "pending",
  "assignee_id": "usr_99x817z",
  "due_date": "2026-04-01T12:00:00Z",
  "created_at": "2026-03-19T19:58:00Z",
  "updated_at": "2026-03-19T19:58:00Z"
}
```

**Error Responses:**

*   **400 Bad Request:** Occurs if the required `title` field is missing or exceeds character limits. Ensure payload matches schema.
*   **401 Unauthorized:** Occurs if the JWT token is invalid.
*   **404 Not Found:** Occurs if the `assignee_id` provided does not exist in the workspace. Verify the user ID.

**Error Example (400 Bad Request):**
```json
{
  "error": {
    "code": "missing_required_field",
    "message": "The 'title' field is required to create a task.",
    "target": "title"
  }
}
```

### GET /tasks/{id}
Retrieves the details of a specific task by its unique identifier. Use this endpoint to fetch up-to-date information for a single record.

**Request Parameters:**

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `id` | string | Required | The unique identifier of the task, passed in the URL path. |

**Request Body:**
*No request body required.*

**Response Example (200 OK):**
```json
{
  "id": "tsk_01h8a9vbc",
  "title": "Update database schema",
  "description": "Add the new user preference columns to the main PostgreSQL database.",
  "status": "in_progress",
  "assignee_id": "usr_99x817z",
  "due_date": "2026-04-15T17:00:00Z",
  "created_at": "2026-03-10T09:15:30Z",
  "updated_at": "2026-03-19T14:22:10Z"
}
```

**Error Responses:**

*   **401 Unauthorized:** Occurs if authentication fails. Check the `Authorization` header.
*   **404 Not Found:** Occurs if the task ID does not exist or you lack permission to view it. Confirm the `id` is correct.
*   **500 Internal Server Error:** Occurs if the API experiences an unexpected fault. Retry the request later.

**Error Example (404 Not Found):**
```json
{
  "error": {
    "code": "resource_not_found",
    "message": "No task exists with the provided ID.",
    "target": "id"
  }
}
```

### PUT /tasks/{id}
Fully updates an existing task. Use this endpoint to modify a task's attributes. Unspecified optional fields will be set to null or their default values.

**Request Parameters:**

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `id` | string | Required | The unique identifier of the task, passed in the URL path. |
| `title` | string | Required | The updated headline or name of the task. Max 100 characters. |
| `description` | string | Optional | The updated detailed explanation of the task. |
| `status` | string | Required | The updated state. Must be `pending`, `in_progress`, or `completed`. |
| `assignee_id` | string | Optional | The updated user identifier responsible for the task. |
| `due_date` | string | Optional | The updated deadline, formatted as an ISO 8601 datetime string. |

**Request Body Example:**
```json
{
  "title": "Update database schema",
  "description": "Add the new user preference columns to the main PostgreSQL database and optimize indexes.",
  "status": "completed",
  "assignee_id": "usr_99x817z",
  "due_date": "2026-04-15T17:00:00Z"
}
```

**Response Example (200 OK):**
```json
{
  "id": "tsk_01h8a9vbc",
  "title": "Update database schema",
  "description": "Add the new user preference columns to the main PostgreSQL database and optimize indexes.",
  "status": "completed",
  "assignee_id": "usr_99x817z",
  "due_date": "2026-04-15T17:00:00Z",
  "created_at": "2026-03-10T09:15:30Z",
  "updated_at": "2026-03-19T20:15:00Z"
}
```

**Error Responses:**

*   **400 Bad Request:** Occurs if the `status` string is invalid. Ensure the value is one of the allowed enums.
*   **404 Not Found:** Occurs if the specified task ID cannot be located. Verify the ID in the path.
*   **409 Conflict:** Occurs if the task is locked by another simultaneous update. Retry the operation.

**Error Example (409 Conflict):**
```json
{
  "error": {
    "code": "concurrent_modification",
    "message": "The task was modified by another user. Fetch the latest version before updating.",
    "target": "id"
  }
}
```

### DELETE /tasks/{id}
Permanently deletes a task. Use this endpoint to remove tasks that are obsolete or created in error. This action cannot be undone.

**Request Parameters:**

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `id` | string | Required | The unique identifier of the task, passed in the URL path. |

**Request Body:**
*No request body required.*

**Response Example (204 No Content):**
*(No body is returned for a successful deletion)*

**Error Responses:**

*   **401 Unauthorized:** Occurs if the request lacks a valid Bearer token.
*   **403 Forbidden:** Occurs if the authenticated user lacks administrative privileges to delete this task.
*   **404 Not Found:** Occurs if the task ID does not exist or has already been deleted.

**Error Example (403 Forbidden):**
```json
{
  "error": {
    "code": "insufficient_permissions",
    "message": "Your user role does not permit deleting tasks assigned to other users.",
    "target": "id"
  }
}
```

## Data Models

### Task Object
The core resource representing a unit of work.

| Field Name | Type | Required/Optional |  | Description |
| --- | --- | --- | --- | --- |
| `id` | string | Required |  | The unique alphanumeric identifier for the task. |
| `title` | string | Required |  | The headline or name of the task. |
| `description` | string | Optional |  | A detailed explanation of the task's requirements. |
| `status` | string | Required |  | The current state. Allowed values: `pending`, `in_progress`, `completed`. |
| `assignee_id` | string | Optional |  | The unique identifier of the user assigned to the task. |
| `due_date` | string | Optional |  | The deadline formatted as an ISO 8601 datetime string. |
| `created_at` | string | Required |  | The timestamp of when the task was generated (ISO 8601). |
| `updated_at` | string | Required |  | The timestamp of when the task was last modified (ISO 8601). |

### Error Object
The standard format returned when an API request fails.

| Field Name | Type | Required/Optional | Description |
|---|---|---|---|
| `error` | object | Required | The top-level wrapper for the error details. |
| `error.code` | string | Required | A machine-readable string indicating the specific error type. |
| `error.message` | string | Required | A human-readable and actionable explanation of what went wrong. |
| `error.target` | string | Optional | The specific parameter or field that caused the error. |

## Rate Limiting
To ensure stability, the Nexus Task API enforces a rate limit of 100 requests per minute per API key. 

Rate limiting status is communicated via the following HTTP response headers:
*   `X-RateLimit-Limit`: The total number of requests permitted per minute (100).
*   `X-RateLimit-Remaining`: The number of requests left in the current 60-second window.
*   `X-RateLimit-Reset`: The Unix timestamp indicating when the limit will reset.

If you exceed the limit, the API will reject further requests and return a `429 Too Many Requests` status code. The response body will contain a standard Error Object. Wait until the time specified in the `X-RateLimit-Reset` header before resuming operations.

## Pagination
Endpoints returning multiple items (e.g., `GET /tasks`) use cursor-based pagination. This prevents data duplication or skipped records if items are added or removed while you are paginating.

To paginate, limit your payload size using the `limit` query parameter. If more results exist beyond the requested limit, the response's `pagination` object will set `has_more` to `true` and provide a `next_cursor` string.

To fetch the next page, pass that exact string into the `cursor` query parameter of your subsequent request. When `has_more` is `false`, you have reached the end of the collection.

## Code Examples
The following examples demonstrate how to create a task using the `POST /tasks` endpoint.

**cURL:**
```bash
curl -X POST "https://api.nexus.com/v1/tasks" \
     -H "Authorization: Bearer <YOUR_JWT_TOKEN>" \
     -H "Content-Type: application/json" \
     -d '{
           "title": "Provision staging servers",
           "description": "Deploy 3 new AWS EC2 instances for the Q3 release staging environment.",
           "status": "pending",
           "assignee_id": "usr_99x817z",
           "due_date": "2026-04-01T12:00:00Z"
         }'
```

**Python (requests):**
```python
import requests
import json

url = "https://api.nexus.com/v1/tasks"
headers = {
    "Authorization": "Bearer <YOUR_JWT_TOKEN>",
    "Content-Type": "application/json"
}
payload = {
    "title": "Provision staging servers",
    "description": "Deploy 3 new AWS EC2 instances for the Q3 release staging environment.",
    "status": "pending",
    "assignee_id": "usr_99x817z",
    "due_date": "2026-04-01T12:00:00Z"
}

response = requests.post(url, headers=headers, data=json.dumps(payload))
print(response.status_code)
print(response.json())
```

**JavaScript (fetch):**
```javascript
const url = "https://api.nexus.com/v1/tasks";
const token = "<YOUR_JWT_TOKEN>";

const payload = {
  title: "Provision staging servers",
  description: "Deploy 3 new AWS EC2 instances for the Q3 release staging environment.",
  status: "pending",
  assignee_id: "usr_99x817z",
  due_date: "2026-04-01T12:00:00Z"
};

fetch(url, {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${token}`,
    "Content-Type": "application/json"
  },
  body: JSON.stringify(payload)
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Error:", error));
```

## Changelog

| Version | Date | Change Description |
|---|---|---|
| v1.2.0 | 2026-02-15 | Added `status` query parameter to the `GET /tasks` endpoint for status filtering. |
| v1.1.0 | 2025-11-04 | Implemented cursor-based pagination for listing tasks. Deprecated offset pagination. |
| v1.0.0 | 2025-06-22 | Initial public release of the Nexus Task API. |

</section>
