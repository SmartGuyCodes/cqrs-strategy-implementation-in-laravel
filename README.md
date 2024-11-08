# Implementing CQRS with Laravel

![Build Status](https://github.com/SmartGuyCodes/cqrs-strategy-implementation-in-laravel/.github/workflows/cci.yml/badge.svg)

<!-- [![Quality Score](https://img.shields.io/SmartGuyCodes/cqrs-strategy-implementation-in-laravel.svg?style=flat-square)](https://github.com/SmartGuyCodes/cqrs-strategy-implementation-in-laravel) -->

This repository demonstrates a Command Query Responsibility Segregation (CQRS) pattern to achieve flexibility, agility, and performance in a web service built using Laravel 11. It features a simple Article API that supports standard CRUD operations and showcases the advantages of CQRS in designing scalable and efficient systems.

---

## Table of Contents

1. Introduction
2. CQRS Benefits
3. Setup Instructions
4. API Endpoints
5. CQRS Pattern Implementation
6. Sample Requests
7. Contributing

---

## Introduction

The CQRS pattern divides an application’s actions into commands (write operations) and queries (read operations). Each type of action has its own handler, allowing read and write operations to be managed independently. This separation is especially useful in applications that require scalability and high performance.

---

### Key Features of This API

1. Article Creation, Update, and Deletion - Command operations are handled by distinct command classes and handlers.
2. Article Retrieval - Query operations are handled by separate query classes and handlers.
3. Standardized API Responses - All responses are formatted consistently using Laravel's JsonResource.

---

## CQRS Benefits
Implementing CQRS brings several benefits, particularly for APIs that need flexibility, agility, and performance:

1. Flexibility - Separates read and write models, allowing them to evolve independently. For example, queries can use more optimized data structures without affecting the write side.
2. Agility - Makes code easier to maintain and extend as different team members can work on commands and queries independently.
3. Performance - Reduces read-write contention by isolating query operations from commands. This structure is beneficial for high-traffic applications.

---

## Setup Instructions

To get started with this project, follow these steps:

### Prerequisites
- PHP 8.2 or later
- Composer
- Laravel 11
- MySQL or another compatible database

### Installation

1. Clone the repository:

```bash
git clone https://github.com/SmartGuyCodes/cqrs-strategy-implementation-in-laravel
```
- Enter the project directory
```bash
cd cqrs-strategy-implementation-in-laravel
```

2. Install dependencies:

```bash
composer install
```

3. Copy .env.example to .env and configure your environment variables.

```bash
cp .env.example .env
```

4. Generate project encryption key

```bash
php artisan key:generate
```

5. Set up the database:

```bash
php artisan migrate
```

6. Start the server:

```bash
php artisan serve
```

---

## API Endpoints

Here is a summary of the available API endpoints.

| Method|          Endpoint  | Description                |
|:------|:------------------:|:---------------------------|
| GET   |  /api/articles     | Retrieve all articles      |
| GET   |  /api/articles/{id}| Retrieve a specific article|
| POST  |  /api/articles     | Create a new article       |
| PUT   |  /api/articles/{id}| Update an existing article |
| DELETE|  /api/articles/{id}| Delete an article          |
		
---

## CQRS Pattern Implementation

The CQRS pattern is applied by splitting the logic for handling commands (create, update, delete) and queries (read operations) into separate classes. Each command and query has its own dedicated handler class, responsible for executing the logic related to that command or query.

### Structure

- ***Commands***: Define the data required for write operations (create, update, delete).
- ***Queries***: Define the data requirements for read operations (retrieve single or multiple records).
- ***Handlers***: Execute the actual logic for each command and query, interacting with models and resources.
- ***Resources***: Standardize API responses.

### Directory Structure

The relevant structure under App/Services/Article is:

```bash
├── Commands
│   ├── CreateArticleCommand.php
│   ├── DeleteArticleCommand.php
│   └── UpdateArticleCommand.php
├── Commands/Handlers
│   ├── CreateArticleHandler.php
│   ├── DeleteArticleHandler.php
│   └── UpdateArticleHandler.php
├── Queries
│   ├── GetAllArticlesQuery.php
│   └── GetArticleByIdQuery.php
├── Queries/Handlers
│   ├── GetAllArticlesHandler.php
│   └── GetArticleByIdHandler.php
```

Each handler returns a structured API response using Laravel Resources.

### Example Workflow

1. Create Article - The CreateArticleCommand is dispatched, calling the CreateArticleHandler to save the article and return a standardized JSON response.
2. Fetch Articles - The GetAllArticlesQuery is dispatched, calling the GetAllArticlesHandler to retrieve and return articles as JSON.

## Sample Requests

Here's how to use each endpoint with sample requests.

### 1. Create an Article

```bash
POST /api/articles
Content-Type: application/json

{
    "title": "New Article",
    "content": "This is a sample article.",
    "author_id": 1
}
```

- Response:

```json
{
    "data": {
        "id": 1,
        "title": "New Article",
        "content": "This is a sample article.",
        "author_id": 1,
        "published_at": "2024-01-01T00:00:00.000000Z"
    }
}
```

### 2. Retrieve All Articles


```bash
GET /api/articles
```

- Response:

```json
{
    "data": [
        {
            "id": 1,
            "title": "New Article",
            "content": "This is a sample article.",
            "author_id": 1,
            "published_at": "2024-01-01T00:00:00.000000Z"
        }
    ]
}
```

### 3. Update an Article

```bash
PUT /api/articles/1
Content-Type: application/json

{
    "title": "Updated Article",
    "content": "Updated content of the article."
}
```

- Response:

```json
{
    "data": {
        "id": 1,
        "title": "Updated Article",
        "content": "Updated content of the article.",
        "author_id": 1,
        "published_at": "2024-01-01T00:00:00.000000Z"
    }
}
```

### 4. Delete an Article

```bash
DELETE /api/articles/1
```

- Response:

```json
{
    "message": "Article deleted successfully"
}
```

---

## Contributing

We welcome contributions! If you find a bug or want to add a feature, please submit an issue or open a pull request.

1. Fork the repository.
2. Create a new branch.
3. Make your changes.
4. Push to your fork and open a pull request.

---

## License

This project is licensed under the MIT License.