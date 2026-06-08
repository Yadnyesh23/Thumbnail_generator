# Project Steps

## 1. Create a Virtual Environment

Create and activate a Python virtual environment to isolate project dependencies.

```bash
python -m venv venv
```

Activate the environment:

### Windows

```bash
venv\Scripts\activate
```

### Linux/macOS

```bash
source venv/bin/activate
```

---

## 2. Install Project Dependencies

Create a `requirements.txt` file and install all required packages.

```bash
pip install -r requirements.txt
```

---

## 3. Configure Environment Variables

Create a `.env` file and add all necessary credentials and configuration values, such as:

* Database URL
* OpenAI API Key
* ImageKit Credentials
* JWT Secret Key
* Other third-party service credentials

Load these values through `config.py` to keep sensitive information centralized and secure.

---

## 4. Connect the Database

Configure the database connection and verify connectivity before proceeding with model creation.

Supported databases:

* PostgreSQL (Recommended)
* MySQL
* SQLite (Development)

---

## 5. Create Database Models

Define all database models using **SQLAlchemy** or **SQLModel**.

### Option A: SQLModel (Recommended)

* Combines SQLAlchemy and Pydantic functionality.
* Supports both ORM and data validation.
* Eliminates the need for separate Pydantic schemas in many cases.

### Option B: SQLAlchemy

* Create ORM models using SQLAlchemy.
* Use separate Pydantic models for request and response validation.

---

## 6. Create Services

### 6.1 ImageKit Service

Responsibilities:

* Upload images to ImageKit.
* Return the uploaded image URL.
* Generate multiple image variants using ImageKit transformations.

Supported variants:

| Variant | Resolution  | Aspect Ratio |
| ------- | ----------- | ------------ |
| YouTube | 1280 × 720  | 16:9         |
| Shorts  | 1080 × 1920 | 9:16         |
| Square  | 1080 × 1080 | 1:1          |

Example output:

```json
{
  "youtube": "https://ik.imagekit.io/.../image.jpg?tr=w-1280,h-720,c-maintain_ratio",
  "shorts": "https://ik.imagekit.io/.../image.jpg?tr=w-1080,h-1920,c-maintain_ratio",
  "square": "https://ik.imagekit.io/.../image.jpg?tr=w-1080,h-1080,c-maintain_ratio"
}
```
