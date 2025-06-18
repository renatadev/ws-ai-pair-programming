# Functional Specification: Kanban Board MVP

## Project Overview

A collaborative Kanban board web application for task management with three columns: "To Do", "Doing", and "Done". Built for a 1-day workshop focusing on human-AI pair programming collaboration.

## Core Features

### F1: Task Display and Board Layout
**User Story**: As a user, I want to see all my tasks organized in three columns so I can track project progress.

**Acceptance Criteria:**
- Display three columns: "To Do", "Doing", "Done"
- Show tasks in their respective columns based on status
- Tasks display title and status only (minimal UI)
- Clean layout using Tailwind CSS
- Board auto-refreshes every 5 seconds to show updates

### F2: Task Creation
**User Story**: As a user, I want to add new tasks to the board so I can track new work items.

**Acceptance Criteria:**
- Simple form with title field only (required)
- New tasks automatically go to "To Do" column
- Basic form validation prevents empty titles
- Task appears immediately after creation

### F3: Task Status Management
**User Story**: As a user, I want to move tasks between columns so I can update their progress status.

**Acceptance Criteria:**
- Click buttons to move tasks: "Start" (To Do → Doing), "Complete" (Doing → Done)
- Status changes persist in database
- Changes visible to other users within 5 seconds

### F4: Task Deletion
**User Story**: As a user, I want to delete tasks that are no longer needed.

**Acceptance Criteria:**
- Simple delete button on each task
- No confirmation prompt (keep it fast)
- Task removed immediately

### F5: Data Persistence
**User Story**: As a user, I want my tasks to be saved so they persist between sessions.

**Acceptance Criteria:**
- All task data stored in PostgreSQL database
- Tasks reload correctly when page is refreshed

## Non-Functional Requirements

### Performance
- Basic performance acceptable for workshop demo
- Task operations complete within 2 seconds
- Polling updates every 5 seconds

### Usability
- Minimal but functional interface
- Desktop-focused (mobile not required)
- Basic error handling only

## Simplified Scope for 4-Hour Development

**Database Schema:**
- Single table: `tasks(id, title, status, created_at)`
- No descriptions, no complex fields

**API Endpoints (4 total):**
- GET /api/tasks
- POST /api/tasks
- PUT /api/tasks/{id}
- DELETE /api/tasks/{id}

**Frontend Components:**
- Board layout (3 columns)
- Task card component
- Add task form
- Move/delete buttons

## Out of Scope

- Task descriptions
- User authentication
- Drag-and-drop functionality
- Advanced error handling
- Complex UI animations
- Task editing after creation
- Confirmation dialogs
- Advanced validation
- Responsive design
- Production deployment

## Success Criteria

The project is successful when:
1. Users can create, view, and delete tasks
2. Tasks move between columns via button clicks
3. Basic real-time updates work via polling
4. Data persists in PostgreSQL
5. Demonstrable in 4-hour timeframe

## Workshop Constraints

- **Must be completable within 4 hours total development time**
- 2 hours backend (database + API)
- 2 hours frontend (UI + integration)
- Should demonstrate effective human-AI collaboration
- Focus on core functionality over polish