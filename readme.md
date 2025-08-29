
# Synkora API

A **RESTful API** for managing **users** and **tasks** in a to-do list application.

This is a **basic but functional backend project** built with **Node.js**, **Express**, and **Firebase**. It follows the **MVC architecture** with **DTOs** for request/response validation and Firestore for data persistence.

> Note: This is my **first backend project**. In the future, I plan to implement additional features, but the timeline is not fixed due to other ongoing projects.

---

## Prerequisites

* **Node.js 18+**
* **Firebase project** configured with Firestore and Firebase Auth
* API client (optional): Postman, Insomnia, or Thunder Client

---

## Environment Configuration

Create a `.env` file in the project root with the following variables:

```
PORT=1234
FIREBASE_API_KEY=your_firebase_api_key
```

> `FIREBASE_API_KEY` is used to authenticate requests to Firebase Auth.

---

## Error Handling

The API returns **standard HTTP status codes** with a **consistent JSON structure**.

### Example errors:

#### 400 Bad Request

**Cause:** Invalid data or malformed request

```json
{
  "message": "Invalid email address",
  "code": "INVALID_EMAIL"
}
```

#### 401 Unauthorized

**Cause:** Token missing or invalid

```json
{
  "message": "Token missing",
  "code": "TOKEN_MISSING"
}
```

```json
{
  "message": "Invalid token",
  "code": "INVALID_TOKEN"
}
```

#### 403 Forbidden

**Cause:** User does not have permission

```json
{
  "message": "Permission denied",
  "code": "PERMISSION_DENIED"
}
```

#### 404 Not Found

**Cause:** Resource does not exist

```json
{
  "message": "Task not found",
  "code": "TASK_NOT_FOUND"
}
```

#### 409 Conflict

**Cause:** Resource already exists

```json
{
  "message": "Email already exists",
  "code": "EMAIL_ALREADY_EXISTS"
}
```

#### 500 Internal Server Error

**Cause:** Unexpected error

```json
{
  "message": "An unexpected error occurred",
  "code": "UNEXPECTED_ERROR"
}
```

---

## API Endpoints

All endpoints are **JSON-based**.
Endpoints that require authentication use the **Bearer token** in the `Authorization` header:

```
Authorization: Bearer <TOKEN>
```

---

## Auth (Authentication)

### 1. POST `/auth/register`

* Registers a new user with email and password.

**Request body:**

```ts
export interface UserRegisterDTO {
    email: string;
    password: string;
}
```

**Response 201:**

```ts
export interface AuthControllerResponseDTO {
    token: string;
}
```

---

### 2. POST `/auth/login`

* Logs in an existing user.

**Request body:**

```ts
export interface UserLoginDTO {
    email: string;
    password: string;
}
```

**Response 200:**

```ts
export interface AuthControllerResponseDTO {
    token: string;
}
```

---

### 3. POST `/auth/google`

* Logs in or registers a user via Google authentication.

**Request body:**

```ts
export interface GoogleAuthDTO {
    uid: string;
    email: string;
}
```

**Response 200:**

```ts
export interface AuthControllerResponseDTO {
    token: string;
}
```

---

## Tasks

> All task endpoints require a valid Bearer token.

### 1. GET `/tasks`

* Returns all tasks of the authenticated user.

**Response 200:**

```ts
export interface TaskControllerResponseDTO {
    tasks: TaskResponseDTO[];
}

export interface TaskResponseDTO {
    id: string;
    title: string;
    description: string;
    completed: boolean;
}
```

---

### 2. POST `/tasks`

* Creates a new task.

**Request body:**

```ts
export interface TaskCreateDTO {
    title: string;
    description: string;
}
```

**Response 201:**

```ts
export interface TaskControllerResponseDTO {
    task: TaskResponseDTO;
}
```

---

### 3. PATCH `/tasks/:id`

* Updates the status of a task (completed or not).

**Request body:**

```ts
export interface TaskUpdateStatusDTO {
    completed: boolean;
}
```

**Response 204 (No Content)**

---

### 4. DELETE `/tasks/:id`

* Deletes a task.

**Response 204 (No Content)**

---

## Running the Project

### Install dependencies

```bash
npm install
```

### Start server

```bash
npm run dev
```

* The server runs on `http://localhost:<PORT>`
* By default, `PORT` is 1234 if not defined in `.env`.

---



