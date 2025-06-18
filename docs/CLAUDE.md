**IMPORTANT FOR CLAUDE: Reference this file before implementing anything**

# Project: Kanban Board MVP

## Project Overview

A collaborative Kanban board web application built for a 4-hour workshop. Features three columns (To Do, Doing, Done) with basic task management functionality. Focus is on demonstrating human-AI pair programming collaboration while building a functional MVP.

## Context Management

### Context refresh

Refresh your memory by checking in `docs/` folder. Then review the `ARCHITECTURE.md` and `FUNCTIONAL.md` to understand what we are building. Check current progress in `TICKETS.md`.

Check what the next ticket that needs implementing.

**Before implementing anything:**

1. Confirm you understand the current ticket requirements
2. Check `TICKETS.md` for any dependencies or related work completed by other tickets
3. Ask if you should reference any specific standards from `CLAUDE.md`
4. Only implement what's specified in this ticket

As you implement, explain:

- How the code works and why it meets our `FUNCTIONAL.md` requirements
- How it aligns with our `ARCHITECTURE.md`
- Why it complies with our standards in `CLAUDE.md`
- Any additional features or components you're implementing beyond the basic ticket requirements

### Context reset

Now we will reset the context window, before we do so, in `docs/` folder:

1. **Update `TICKETS.md`** for the ticket just completed:

   - Mark ticket as ✅ COMPLETE
   - Add any additional features/components implemented beyond original scope
   - Note any tasks from other tickets that were completed during this work
   - Update dependencies if this work enables other tickets to proceed
   - Add any important implementation notes or decisions

2. Create/update `HISTORY_[NAME].md` file summarising our progress:

   - List completed tickets with key implementation details
   - Note any important decisions or patterns established
   - Mention any deviations from original specs and why
   - Save current state of key variables/configurations

3. If applicable, update `CLAUDE.md` with any learned standards picked up from the review process

4. If there have been significant changes, update `FUNCTIONAL.md` or `ARCHITECTURE.md` as required

5. **IMPORTANT**: Be concise, don't repeat yourself, double check and remove duplication/reduce where possible

After updating these files, I'll reset the context window and we'll continue with a fresh session.

---

**NOTE FOR CLAUDE: Fill in the below**

## Tech Stack

- Languages: TypeScript (frontend), Python (backend)
- Frontend: SvelteKit + Tailwind CSS + npm
- Backend: FastAPI + pg8000 + PostgreSQL + pipenv
- Database: PostgreSQL (single tasks table)
- Development: Feature branches with basic commit messages

## Code Style & Conventions

### Import/Module Standards

- TypeScript: ES6 imports, group by external/internal
- Python: Standard library first, then third-party, then local imports
- Keep imports minimal and focused

### Naming Conventions

- Functions: camelCase (TS), snake_case (Python)
- Components: PascalCase (TaskCard.svelte)
- Constants: UPPER_SNAKE_CASE
- Files: kebab-case for components, snake_case for Python modules
- Database: snake_case for tables and columns

### Patterns to Follow

- Functional programming approach (no classes unless necessary)
- Single responsibility principle for functions
- Basic error handling with HTTP status codes
- Direct database queries (no ORM complexity)
- Simple state management in Svelte stores

## Development Workflow

- Branch strategy: Feature branches (frontend-*, backend-*)
- Commit message format: Basic descriptive messages ("add task creation", "fix status update")
- PR requirements: None (workshop setting, merge when ready)
- Integration: Coordinate via shared main branch

## Testing Strategy

- Backend: pytest for 2 critical endpoints only (GET /tasks, POST /tasks)
- Frontend: Manual testing only (no automated tests)
- Coverage: Focus on core functionality, not comprehensive
- Test naming: test_[function_name] for Python

## Environment Setup

- Required environment variables: DATABASE_URL (PostgreSQL connection string)
- Backend setup: `cd backend && pipenv install && pipenv shell`
- Frontend setup: `cd frontend && npm install`
- Database: Local PostgreSQL instance, create database 'kanban_db'

## Common Commands

```bash
# Backend development server
cd backend && pipenv run uvicorn main:app --reload --port 8000

# Frontend development server  
cd frontend && npm run dev

# Backend tests
cd backend && pipenv run pytest

# Frontend build
cd frontend && npm run build

# Database setup
createdb kanban_db
```

## Project Structure

Key directories and their purpose:

- `/backend` - FastAPI application with modular structure
- `/frontend` - SvelteKit application with component-based architecture  
- `/docs` - Project specifications and documentation
- `/backend/src` - API routes and business logic
- `/backend/db` - Database connection and queries
- `/frontend/src/lib` - Reusable components and utilities

## Review Process Guidelines

Before submitting any code, ensure the following steps are completed:

1. **Run relevant commands**:
   - Backend: `pipenv run pytest` (for the 2 test endpoints)
   - Frontend: `npm run build` (to verify no TypeScript errors)

2. **Workshop-focused review**:
   - Code is functional and demonstrates the feature
   - Basic error handling is in place
   - Code is readable and understandable for workshop demo

3. **Minimal compliance check**:
   - ✅ Follows basic naming conventions
   - ✅ Uses specified tech stack correctly
   - ✅ Integrates with existing code structure
   - ✅ Meets functional requirements from FUNCTIONAL.md

4. **Workshop checklist**:
   - [ ] Feature works as expected
   - [ ] Code is demo-ready
   - [ ] No blocking errors
   - [ ] Ready for integration with teammate's work

## Known Issues & Workarounds

Workshop-specific limitations to be aware of:

- No authentication system (tasks are public)
- Basic error handling only (sufficient for demo)
- Polling instead of WebSockets (simpler implementation)
- No data validation beyond basic requirements
- Manual database setup required (no migrations)

## References

- [SvelteKit Documentation](https://kit.svelte.dev/docs)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [pg8000 Documentation](https://github.com/tlocke/pg8000)
- Workshop specifications: `FUNCTIONAL.md` and `ARCHITECTURE.md`
