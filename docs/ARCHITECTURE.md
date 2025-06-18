# Architectural Specification: Kanban Board MVP

## System Overview

A simple full-stack web application with a TypeScript SvelteKit frontend and Python FastAPI backend, using PostgreSQL for data persistence.

## Technology Stack

### Frontend
- **Framework**: SvelteKit with TypeScript
- **Styling**: Tailwind CSS (utility-first)
- **Package Manager**: npm
- **Build Tool**: Vite (included with SvelteKit)

### Backend
- **Framework**: FastAPI (Python)
- **Database Driver**: pg8000 (pure Python PostgreSQL driver)
- **Package Manager**: pipenv
- **Development Server**: uvicorn

### Database
- **System**: PostgreSQL
- **Schema**: Single table design
- **Connection**: Direct connection via pg8000

## System Architecture

```
┌─────────────────┐    HTTP/JSON    ┌─────────────────┐    SQL    ┌──────────────┐
│   SvelteKit     │ ────────────→  │    FastAPI      │ ────────→ │ PostgreSQL   │
│   Frontend      │                │    Backend      │           │   Database   │
│   (Port 5173)   │ ←──────────────│   (Port 8000)   │ ←──────── │   (Port 5432)│
└─────────────────┘                └─────────────────┘           └──────────────┘
```

## Database Schema

### Tasks Table
```sql
CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'todo',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Status values: 'todo', 'doing', 'done'
```

### Sample Data
```sql
INSERT INTO tasks (title, status) VALUES 
('Set up project structure', 'done'),
('Create API endpoints', 'doing'),
('Build frontend components', 'todo');
```

## API Specification

### Base URL
`http://localhost:8000/api`

### Endpoints

#### GET /tasks
**Purpose**: Retrieve all tasks
**Response**: 
```json
[
  {
    "id": 1,
    "title": "Task title",
    "status": "todo",
    "created_at": "2024-01-01T10:00:00"
  }
]
```

#### POST /tasks
**Purpose**: Create new task
**Request Body**:
```json
{
  "title": "New task title"
}
```
**Response**: 
```json
{
  "id": 2,
  "title": "New task title", 
  "status": "todo",
  "created_at": "2024-01-01T10:05:00"
}
```

#### PUT /tasks/{id}
**Purpose**: Update task status
**Request Body**:
```json
{
  "status": "doing"
}
```
**Response**: 
```json
{
  "id": 1,
  "title": "Task title",
  "status": "doing", 
  "created_at": "2024-01-01T10:00:00"
}
```

#### DELETE /tasks/{id}
**Purpose**: Delete task
**Response**: 
```json
{
  "message": "Task deleted"
}
```

## Project Structure

### Backend Structure
```
backend/
├── main.py              # FastAPI app entry point
├── src/
│   ├── __init__.py
│   ├── api/
│   │   ├── __init__.py
│   │   └── routes.py    # All API endpoints
│   └── models/
│       ├── __init__.py
│       └── schemas.py   # Pydantic request/response models
├── db/
│   ├── __init__.py
│   ├── connection.py    # Database connection logic
│   └── queries.py       # SQL queries
├── tests/
│   ├── __init__.py
│   └── test_api.py      # Basic API tests (2 endpoints)
├── Pipfile              # Dependencies
└── README.md
```

### Frontend Structure
```
frontend/
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── TaskCard.svelte
│   │   │   ├── TaskForm.svelte
│   │   │   └── KanbanBoard.svelte
│   │   └── api/
│   │       └── tasks.ts     # API client functions
│   ├── routes/
│   │   └── +page.svelte     # Main board page
│   └── app.html
├── static/
├── package.json
└── README.md
```

## Data Flow

### Task Creation Flow
1. User fills form in `TaskForm.svelte`
2. Frontend calls `POST /api/tasks` via `tasks.ts`
3. FastAPI validates request using Pydantic model
4. Backend executes SQL INSERT via `queries.py`
5. Database returns new task with ID
6. API returns task data to frontend
7. Frontend updates local state and re-renders

### Real-time Updates Flow
1. `KanbanBoard.svelte` runs `setInterval()` every 5 seconds
2. Calls `GET /api/tasks` via `tasks.ts`
3. Backend queries database for all tasks
4. Frontend compares with current state
5. Updates UI if changes detected

### Task Status Update Flow
1. User clicks move button on `TaskCard.svelte`
2. Frontend calls `PUT /api/tasks/{id}` with new status
3. Backend updates database via SQL UPDATE
4. Frontend immediately updates local state
5. Next polling cycle confirms change

## Deployment Architecture

### Development Environment
- Frontend: `npm run dev` (localhost:5173)
- Backend: `uvicorn main:app --reload` (localhost:8000)
- Database: Local PostgreSQL instance

### Database Connection
```python
# Simple connection pattern
import pg8000
conn = pg8000.connect(
    host="localhost",
    database="kanban_db",
    user="your_user",
    password="your_password"
)
```

## Error Handling Strategy

### Backend Error Responses
- **400**: Bad request (validation errors)
- **404**: Task not found
- **500**: Server error

### Frontend Error Handling
- Basic try-catch around API calls
- Simple error messages in UI
- Graceful degradation if backend unavailable

## Performance Considerations

### Optimizations for 4-Hour Scope
- No connection pooling (direct connections)
- No caching layer
- Simple polling instead of WebSockets
- Minimal error handling
- No request rate limiting

### Acceptable Limitations
- Database connections not optimized
- No horizontal scaling considerations
- Basic security (no authentication)
- No data validation beyond basic requirements

## Security Considerations

### Minimal Security for Workshop
- Input validation via Pydantic models
- Basic SQL injection prevention via parameterized queries
- CORS configured for development
- No authentication/authorization required

## Testing Strategy

### Backend Testing
- Test 2 critical endpoints: GET /tasks and POST /tasks
- Use pytest with simple test cases
- No complex mocking or fixtures

### Frontend Testing
- Manual testing only
- No automated frontend tests

## Integration Points

### Frontend-Backend Integration
- RESTful API communication
- JSON data exchange
- CORS configured for localhost development
- Error handling for network failures

### Database Integration
- Direct SQL queries (no ORM)
- Simple connection management
- Basic error handling for database failures