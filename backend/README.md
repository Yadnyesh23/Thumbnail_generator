# Tech Stack

## Backend

* **FastAPI** - High-performance Python web framework for building APIs.
* **SQLModel** - ORM built on top of SQLAlchemy and Pydantic.
* **SQLite** - Lightweight database for development and testing.

## Frontend

* **React** - Component-based UI library.
* **Vite** - Fast frontend build tool and development server.
* **CSS** - Styling and responsive layouts.

## AI

* **OpenAI** - Generates thumbnail concepts, prompts, and image variations.

## Storage & CDN

* **ImageKit**

  * Image upload and storage
  * CDN delivery
  * URL-based image transformations
  * Dynamic resizing and optimization

## Real-Time Communication

* **Server-Sent Events (SSE)**

  * Streams thumbnail generation progress to the frontend in real time.
  * Provides live status updates without polling.

---

# Database Models

## 1. Job

Represents a thumbnail generation request.

| Field          | Type     | Description                      |
| -------------- | -------- | -------------------------------- |
| id             | UUID     | Unique identifier                |
| prompt         | String   | User prompt                      |
| num_thumbnails | Integer  | Number of thumbnails to generate |
| headshot_url   | String   | Uploaded user image URL          |
| status         | String   | Current job status               |
| created_at     | DateTime | Creation timestamp               |

### Relationship

```python
thumbnails: List["Thumbnail"]
```

A single Job can have multiple generated Thumbnails.

---

## 2. Thumbnail

Represents an individual generated thumbnail.

| Field         | Type     | Description            |
| ------------- | -------- | ---------------------- |
| id            | UUID     | Unique identifier      |
| job_id        | UUID     | Foreign key to Job     |
| style_name    | String   | Thumbnail style        |
| status        | String   | Generation status      |
| error_message | String   | Error details (if any) |
| created_at    | DateTime | Creation timestamp     |

### Relationship

```python
job: Optional["Job"]
```

Each Thumbnail belongs to one Job.

---

# ORM & Validation

## What is SQLModel?

SQLModel is a modern ORM created by the author of FastAPI.

### Features

* Built on top of SQLAlchemy
* Uses Pydantic for validation
* Type-safe models
* Less boilerplate code
* Supports both ORM and API schemas

Example:

```python
class Job(SQLModel, table=True):
    id: str
    prompt: str
```

### Advantages

* Single model for database + validation
* Better FastAPI integration
* Cleaner codebase
* Easier learning curve

---

## What is SQLAlchemy?

SQLAlchemy is the most widely used Python ORM.

### Features

* Full database abstraction
* Powerful querying capabilities
* Supports complex relationships
* Industry standard ORM

Example:

```python
class Job(Base):
    __tablename__ = "jobs"

    id = Column(String, primary_key=True)
    prompt = Column(String)
```

### Advantages

* Highly flexible
* Production proven
* Extensive ecosystem

### Drawback

Requires separate Pydantic schemas for request and response validation.

---

## What are Pydantic Models?

Pydantic models are used for:

* Request validation
* Response serialization
* Data parsing
* Type checking

Example:

```python
class JobCreate(BaseModel):
    prompt: str
    num_thumbnails: int
```

### Why Use Pydantic?

* Automatic validation
* Better API documentation
* Type safety
* Error handling

---

## SQLModel vs SQLAlchemy

| Feature             | SQLModel    | SQLAlchemy          |
| ------------------- | ----------- | ------------------- |
| ORM                 | ✅           | ✅                   |
| Validation          | ✅ Built-in  | ❌ Requires Pydantic |
| FastAPI Integration | ✅ Excellent | ✅ Good              |
| Boilerplate         | Less        | More                |
| Learning Curve      | Easier      | Moderate            |
| Flexibility         | Moderate    | High                |

### Recommendation

For this project, **SQLModel** is recommended because it combines:

* SQLAlchemy (ORM)
* Pydantic (Validation)
* FastAPI Integration

into a single, clean developer experience.
