# Java Backend Developer Assessment

## Overview

Build a **Personal Book Tracker** REST API using Spring Boot. The application should allow users to search for books via the [OpenLibrary API](https://openlibrary.org/developers/api) and manage a personal reading list stored in a local database.

You have **1 hour** to complete this assessment. Focus on getting the core requirements working cleanly. Bonus tasks are optional and only if time permits.

### How it works

The application has two distinct concerns:
- **Search** — queries the live OpenLibrary API and returns results. Nothing is saved.
- **Reading List** — a locally persisted library the user curates. Books are added manually and managed via CRUD operations.

The intended user journey is: search for a book via the public API, then save the desired result to your local reading list. The two flows are intentionally decoupled — the API does not enforce that a saved book originated from a search result.

---

## Getting Started

- Scaffold a new Spring Boot project from scratch. You are free to use any version of Java, and either Maven or Gradle as your build tool.
- You are free to use [start.spring.io](https://start.spring.io) to bootstrap the project.
- Use **H2** as the embedded database (no external DB setup required).
- Use of AI Coding Assistant is allowed.

---

## The OpenLibrary Search API

You will use the following public API — no authentication or API key required.

```
GET https://openlibrary.org/search.json?q={query}
```

**Example:** `https://openlibrary.org/search.json?q=the+great+gatsby`

Relevant fields in the response:
- `title`
- `author_name` (array)
- `first_publish_year`
- `isbn` (array)

---

## Requirements

### 1. Search Books
- `GET /api/books/search?query={query}`
- This is your internal endpoint. It must internally call the OpenLibrary API using **FeignClient** or **RestTemplate**, map the response to only the relevant fields listed below, and return the mapped results to the client.
- Results are **not** persisted — this is a live search only.

### 2. Save a Book
- `POST /api/books`
- Saves a book to the local reading list.
- Request body must include at minimum: `title`, `author`, `publishYear`, `isbn`.
- All fields should be validated — reject invalid or incomplete requests with a meaningful error response.

### 3. List Saved Books
- `GET /api/books`
- Returns all books currently saved in the reading list.

### 4. Get a Book
- `GET /api/books/{id}`
- Returns a single saved book by its ID.
- Return a meaningful error if the book is not found.

### 5. Update Reading Status & Notes
- `PUT /api/books/{id}`
- Allows updating the following fields only:
  - `notes` — free text notes about the book
  - `readStatus` — one of: `WANT_TO_READ`, `READING`, `COMPLETED`
- Both fields are optional in the request, but at least one should be present.

### 6. Delete a Book
- `DELETE /api/books/{id}`
- Removes a book from the reading list.
- Return a meaningful error if the book is not found.

---

## Technical Expectations

- Use **FeignClient** or **RestTemplate** to integrate with the OpenLibrary API.
- Use **Spring Data JPA** with **H2** for persistence.
- Apply input validation using `@Valid` and appropriate annotations.
- Handle errors gracefully — clients should receive structured, consistent error responses (not stack traces).
- Write clean, readable code. Structure, code organized in packages and naming matter.

---

## Bonus (Optional)

Only attempt these if you have completed all requirements above.

- Write **unit tests** for your business logic. Mock external dependencies (the Feign client, the repository) rather than testing them directly.

---

## Submission

1. You will receive access to a GitHub repository with this README on the `main` branch.
2. Cut a **feature branch** from `main` (name it however you like).
3. Complete the assignment on your feature branch.
4. When done, open a **Pull Request** from your feature branch into `main`.
5. Do not merge the PR — the reviewers will evaluate it as-is.

---

## Evaluation Criteria

| Area | What we look at |
|---|---|
| Correctness | Do the endpoints work as described? |
| Code Quality | Is the code readable, well-structured, and consistent? |
| API Integration | Is the external API called correctly and responses mapped cleanly? |
| Persistence | Is JPA used correctly? Is the schema sensible? |
| Validation & Error Handling | Are bad inputs rejected? Are errors informative? |
| Bonus | Unit tests are a plus, not a requirement |