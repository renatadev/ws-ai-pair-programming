# Kanban Board MVP - Development Tickets

## Ticket Order & Dependencies

Tickets are ordered to maximize parallel development while respecting dependencies. Both developers can work simultaneously on their assigned tickets.

---

## Phase 1: Foundation (Parallel Setup)

### T01: Backend Project Setup
**Assignee:** Adel  
**Dependencies:** None  

**Description:** Set up backend project structure and database foundation

**Deliverables:**
- [ ] Backend directory structure (`main.py`, `src/`, `db/`, `tests/`)
- [ ] Pipfile with dependencies (FastAPI, pg8000, pytest, uvicorn)
- [ ] Database connection module (`db/connection.py`)
- [ ] PostgreSQL database setup with tasks table
- [ ] Basic FastAPI app structure

**Definition of Done:**
- Backend runs with `pipenv run uvicorn main:app --reload`
- Database connection established
- Tasks table created with correct schema
- Health check endpoint responds

**Status Tracking:**
- [ ] Started
- [ ] Database schema created
- [ ] Dependencies installed
- [ ] Basic FastAPI app running
- [ ] ✅ COMPLETE

---

### T02: Frontend Project Setup
**Assignee:** Renata  
**Dependencies:** None  

**Description:** Set up frontend project structure and basic layout

**Deliverables:**
- [ ] SvelteKit project with TypeScript
- [ ] Tailwind CSS configuration
- [ ] Frontend directory structure (`src/lib/components/`, `src/lib/api/`)
- [ ] Basic page layout with 3-column structure
- [ ] API client module setup

**Definition of Done:**
- Frontend runs with `npm run dev`
- 3-column layout visible
- Tailwind CSS working
- TypeScript compilation successful

**Status Tracking:**
- [ ] Started
- [ ] SvelteKit + TypeScript initialized
- [ ] Tailwind configured
- [ ] Basic layout created
- [ ] ✅ COMPLETE

---

## Phase 2: Core Backend API (Sequential)

### T03: Task Model & Database Queries
**Assignee:** Adel  
**Dependencies:** T01 (Backend Setup)  

**Description:** Implement task data model and database operations

**Deliverables:**
- [ ] Pydantic models for task requests/responses (`src/models/schemas.py`)
- [ ] Database query functions (`db/queries.py`)
- [ ] CRUD operations: create, read, update, delete
- [ ] Basic error handling for database operations

**Definition of Done:**
- All CRUD operations work against database
- Pydantic models validate correctly
- Database errors handled gracefully

**Status Tracking:**
- [ ] Started
- [ ] Pydantic models created
- [ ] Database queries implemented
- [ ] Error handling added
- [ ] ✅ COMPLETE

---

### T04: REST API Endpoints
**Assignee:** Adel  
**Dependencies:** T03 (Task Model)  

**Description:** Implement all API endpoints with proper HTTP methods

**Deliverables:**
- [ ] GET /api/tasks - retrieve all tasks
- [ ] POST /api/tasks - create new task
- [ ] PUT /api/tasks/{id} - update task status
- [ ] DELETE /api/tasks/{id} - delete task
- [ ] CORS configuration for frontend (see code below)
- [ ] Basic request validation

**CORS Setup Code for Adel:**
```python
# Add to main.py
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Add CORS middleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],  # Renata's frontend dev server
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

**Definition of Done:**
- All endpoints respond correctly
- CORS allows frontend connections
- Request/response formats match architecture spec
- Basic validation prevents invalid data

**Status Tracking:**
- [ ] Started
- [ ] GET /api/tasks implemented
- [ ] POST /api/tasks implemented
- [ ] PUT /api/tasks/{id} implemented
- [ ] DELETE /api/tasks/{id} implemented
- [ ] CORS configured
- [ ] ✅ COMPLETE

---

## Phase 3: Core Frontend Components (Can Start After T02)

### T05: Task Card Component
**Assignee:** Renata  
**Dependencies:** T02 (Frontend Setup)  

**Description:** Create reusable task card component with actions

**Deliverables:**
- [ ] TaskCard.svelte component
- [ ] Display task title and status
- [ ] Action buttons: Start, Complete, Delete
- [ ] Tailwind styling for card layout
- [ ] Event handlers for button clicks

**Definition of Done:**
- Task card displays correctly
- Buttons trigger events (even without backend)
- Responsive design with Tailwind
- Component accepts task props

**Status Tracking:**
- [ ] Started
- [ ] Component structure created
- [ ] Styling applied
- [ ] Event handlers added
- [ ] ✅ COMPLETE

---

### T06: Task Form Component
**Assignee:** Renata  
**Dependencies:** T02 (Frontend Setup)  

**Description:** Create form for adding new tasks

**Deliverables:**
- [ ] TaskForm.svelte component
- [ ] Input field for task title
- [ ] Submit button
- [ ] Basic form validation (required title)
- [ ] Clear form after submission
- [ ] Tailwind styling for form

**Definition of Done:**
- Form submits with valid data
- Validation prevents empty submissions
- Form resets after successful submission
- Accessible form structure

**Status Tracking:**
- [ ] Started
- [ ] Form structure created
- [ ] Validation implemented
- [ ] Styling applied
- [ ] ✅ COMPLETE

---

## Phase 4: Board Layout & Integration (Requires T04, T05, T06)

### T07: Kanban Board Layout
**Assignee:** Renata  
**Dependencies:** T05 (Task Card), T06 (Task Form)  

**Description:** Implement main board layout with columns and task organization

**Deliverables:**
- [ ] KanbanBoard.svelte component
- [ ] Three columns: To Do, Doing, Done
- [ ] Task filtering by status
- [ ] Responsive column layout
- [ ] Integration of TaskCard and TaskForm components

**Definition of Done:**
- Tasks display in correct columns
- Columns scale with content
- Components integrate smoothly
- Layout works on different screen sizes

**Status Tracking:**
- [ ] Started
- [ ] Column layout created
- [ ] Task filtering implemented
- [ ] Components integrated
- [ ] ✅ COMPLETE

---

### T08: API Integration
**Assignee:** Renata  
**Dependencies:** T04 (API Endpoints), T07 (Board Layout)  

**Description:** Connect frontend to backend API

**Deliverables:**
- [ ] API client functions (`src/lib/api/tasks.ts`)
- [ ] Fetch tasks from backend
- [ ] Create new tasks via API
- [ ] Update task status via API
- [ ] Delete tasks via API
- [ ] Error handling for API failures

**Definition of Done:**
- All CRUD operations work through API
- Frontend updates immediately on actions
- API errors displayed to user
- Network failures handled gracefully

**Status Tracking:**
- [ ] Started
- [ ] API client created
- [ ] GET requests implemented
- [ ] POST requests implemented
- [ ] PUT requests implemented
- [ ] DELETE requests implemented
- [ ] Error handling added
- [ ] ✅ COMPLETE

---

## Phase 5: Real-time Updates & Polish

### T09: Auto-refresh Implementation
**Assignee:** Renata  
**Dependencies:** T08 (API Integration)  

**Description:** Implement polling for real-time updates

**Deliverables:**
- [ ] Polling mechanism every 5 seconds
- [ ] Update board without full page reload
- [ ] Prevent polling conflicts with user actions
- [ ] Loading states during updates

**Definition of Done:**
- Board updates automatically every 5 seconds
- User actions don't conflict with polling
- Loading indicators show during updates
- Polling starts/stops appropriately

**Status Tracking:**
- [ ] Started
- [ ] Polling implemented
- [ ] Conflict prevention added
- [ ] Loading states added
- [ ] ✅ COMPLETE

---

### T10: Basic Testing
**Assignee:** Adel  
**Dependencies:** T04 (API Endpoints)  

**Description:** Implement basic backend tests for critical endpoints

**Deliverables:**
- [ ] Test setup with pytest
- [ ] Test GET /api/tasks endpoint
- [ ] Test POST /api/tasks endpoint
- [ ] Test database integration
- [ ] Test running documentation

**Definition of Done:**
- Tests run with `pipenv run pytest`
- Both critical endpoints tested
- Tests pass consistently
- Database tests include cleanup

**Status Tracking:**
- [ ] Started
- [ ] Test setup created
- [ ] GET endpoint tests
- [ ] POST endpoint tests
- [ ] Database integration tests
- [ ] ✅ COMPLETE

---

## Phase 6: Final Integration & Demo Prep

### T11: End-to-End Integration Testing
**Assignee:** Both (Renata + Adel)  
**Dependencies:** T09 (Auto-refresh), T10 (Basic Testing)  

**Description:** Test complete system integration and prepare for demo

**Deliverables:**
- [ ] Full workflow testing (create → move → delete)
- [ ] Real-time updates verification
- [ ] Error scenarios testing
- [ ] Performance check under normal load
- [ ] Demo data preparation

**Definition of Done:**
- All features work together seamlessly
- Real-time updates function properly
- Basic error handling works
- System ready for demo

**Status Tracking:**
- [ ] Started
- [ ] Full workflow tested
- [ ] Real-time updates verified
- [ ] Error scenarios tested
- [ ] Demo data prepared
- [ ] ✅ COMPLETE

---

## Status Template for Each Ticket

When working on a ticket, update the following:

### Additional Features Implemented
- [ ] Feature 1: Description
- [ ] Feature 2: Description

### Cross-Ticket Dependencies Resolved
- [ ] Dependency 1: Description
- [ ] Dependency 2: Description

### Implementation Notes
- Decision 1: Rationale
- Decision 2: Rationale

### Blockers/Issues
- [ ] Issue 1: Description and status
- [ ] Issue 2: Description and status

---

## Development Workflow

1. **Before Starting a Ticket:**
   - Check TICKETS.md for dependencies
   - Confirm all prerequisite tickets are complete
   - Review FUNCTIONAL.md and ARCHITECTURE.md for requirements

2. **While Working:**
   - Update ticket status in real-time
   - Note any additional features implemented
   - Document important decisions

3. **When Completing:**
   - Mark ticket as ✅ COMPLETE
   - Update any tickets that can now proceed
   - Note any new dependencies discovered

4. **Communication:**
   - Coordinate with your pair on shared dependencies
   - Update TICKETS.md immediately when status changes
   - Flag any blockers that affect the other developer

---

## Completion Tracking

**Total Tickets:** 11  
**Backend Tickets (Adel):** 4 (T01, T03, T04, T10)  
**Frontend Tickets (Renata):** 6 (T02, T05, T06, T07, T08, T09)  
**Shared Tickets:** 1 (T11)  

**Estimated Time Distribution:**
- Backend: ~2 hours
- Frontend: ~2 hours  
- Integration: ~30 minutes
