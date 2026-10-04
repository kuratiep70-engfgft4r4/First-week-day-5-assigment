Here is a complete solution for both exercises formatted according to professional technical writing standards.

---

## Part A: User Manual Procedure

# Setting Up a GitHub Repository and Making Your First Commit

This guide will walk you through creating a new remote repository on GitHub, cloning it to your local machine, and making your very first commit using Git.

### Prerequisites

Before starting this tutorial, ensure you have:

* A computer connected to the internet.
* Git installed on your system.
* An active [GitHub account](https://github.com).
* A terminal application installed (Command Prompt/PowerShell on Windows, or Terminal on macOS/Linux).

---

### Step-by-Step Procedure

1. **Log in to GitHub.**
Open your web browser, navigate to `[https://github.com](https://github.com)`, and log in to your account.
*Expected Result:* You are redirected to your GitHub dashboard.
2. **Create a new repository.**
In the upper-right corner of the page, click the **+** icon next to your profile picture and select **New repository**.
*Expected Result:* The "Create a new repository" form page loads.
3. **Configure repository details.**
In the **Repository name** field, type `my-first-repo`, select **Public**, check the box that says **Add a README file**, and click the green **Create repository** button.
*Expected Result:* GitHub creates the repository and redirects you to the repository's main page showing the default `README.md` file.
> **Screenshot Description:**
> *Image Title:* GitHub Repository Creation Page
> *Content:* A screenshot of the GitHub "Create a new repository" configuration interface. The "Repository name" field highlights `my-first-repo` filled in, the "Public" radio button is selected, the "Add a README file" checkbox is checked with a red callout box, and the green "Create repository" button at the bottom is highlighted.


4. **Copy the repository URL.**
Click the green **Code** button on the repository page and copy the HTTPS URL provided (e.g., `[https://github.com/your-username/my-first-repo.git](https://github.com/your-username/my-first-repo.git)`).
*Expected Result:* The repository URL is saved to your system clipboard.
5. **Clone the repository locally.**
Open your terminal, navigate to your desired working directory, type `git clone <copied-url>`, and press **Enter**.
*Expected Result:* Git downloads a local copy of the repository into a folder named `my-first-repo`.
6. **Navigate into the project directory.**
In your terminal, type `cd my-first-repo` and press **Enter**.
*Expected Result:* Your terminal working directory changes to the `my-first-repo` folder.
7. **Create a new file.**
In your terminal, type `echo "Hello, GitHub!" > hello.txt` and press **Enter**.
*Expected Result:* A new file named `hello.txt` containing the text "Hello, GitHub!" is created inside the directory.
8. **Stage the file.**
Type `git add hello.txt` in your terminal and press **Enter**.
*Expected Result:* The file `hello.txt` is staged for tracking by Git.
9. **Commit the changes.**
Type `git commit -m "Add hello.txt file"` and press **Enter**.
*Expected Result:* Git saves the staged changes locally and displays a summary message confirming 1 file changed.
10. **Push changes to GitHub.**
Type `git push origin main` and press **Enter**.
*Expected Result:* Terminal displays transfer progress metrics ending with `To [https://github.com/](https://github.com/)... [new branch] main -> main`. Refreshing your repository on GitHub reveals `hello.txt` in the file list.

---

### Troubleshooting

* **Error:** `fatal: Authentication failed for...` or `Permission denied (publickey)`.
* **Cause:** Git cannot verify your user identity on GitHub via HTTPS without proper authentication tokens.
* **Solution:** GitHub no longer accepts account passwords for terminal Git operations. Create a **Personal Access Token (PAT)** under **GitHub Settings > Developer Settings > Personal Access Tokens** and use that token instead of your standard login password when prompted by the terminal.

---

## Part B: API Reference Entry

# Endpoint: Create Task

Creates a new task record within a specified project in the application.

### HTTP Request

`POST /api/v1/projects/{project_id}/tasks`

---

### Description

This endpoint allows an authenticated user to create a new task assigned to an existing project. Upon creation, the server validates input fields, assigns a unique task ID, tracks the creator, and records timestamp data.

---

### Request Headers

| Header | Type | Required | Description |
| --- | --- | --- | --- |
| `Authorization` | String | **Yes** | Bearer authentication token in format: `Bearer <your_token>` |
| `Content-Type` | String | **Yes** | Target media type format. Must be `application/json` |

---

### Parameters

#### Path Parameters

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `project_id` | String (UUID) | **Yes** | The unique identifier of the project where the task will be created. |

#### Request Body Parameters

| Field Name | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | String | **Yes** | A short, descriptive title of the task (max 200 characters). |
| `description` | String | **No** | Detailed explanation or context for the task. |
| `assignee_id` | String (UUID) | **No** | The user ID of the team member assigned to the task. |
| `due_date` | String (ISO 8601) | **No** | The deadline date in standard ISO format (`YYYY-MM-DD`). |
| `priority` | String | **No** | Priority level of task. Allowed values: `low`, `medium`, `high`. Default: `medium`. |

---

### HTTP Response Codes

| Status Code | Description |
| --- | --- |
| `201 Created` | The task was successfully created and saved. |
| `400 Bad Request` | Malformed JSON syntax or missing/invalid required fields (e.g., empty `title` or invalid `priority` value). |
| `401 Unauthorized` | Missing, invalid, or expired authentication token. |
| `403 Forbidden` | Authenticated user lacks permission to modify or add tasks to the target project. |
| `404 Not Found` | The specified `project_id` or `assignee_id` does not exist in the database. |
| `500 Internal Server Error` | An unexpected server error occurred while processing the request. |

---

### Request Example

```json
POST /api/v1/projects/550e8400-e29b-41d4-a716-446655440000/tasks
Host: api.taskmaster.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json

{
  "title": "Design Database Schema",
  "description": "Draft initial entity relationship diagram for the healthcare portal backend.",
  "assignee_id": "8f3c9a10-4b21-4d3e-9b6d-112233445566",
  "due_date": "2026-10-15",
  "priority": "high"
}

```

---

### Response Example

**Status:** `201 Created`

```json
{
  "status": "success",
  "message": "Task created successfully.",
  "data": {
    "id": "c7a8b9d0-1e2f-3a4b-5c6d-7e8f9a0b1c2d",
    "project_id": "550e8400-e29b-41d4-a716-446655440000",
    "title": "Design Database Schema",
    "description": "Draft initial entity relationship diagram for the healthcare portal backend.",
    "assignee_id": "8f3c9a10-4b21-4d3e-9b6d-112233445566",
    "due_date": "2026-10-15",
    "priority": "high",
    "created_by": "9a8b7c6d-5e4f-3a2b-1c0d-112233445566",
    "created_at": "2026-10-04T17:43:00Z",
    "updated_at": "2026-10-04T17:43:00Z"
  }
}

```