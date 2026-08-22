# Property Rental Management System

NestJS backend for managing properties, units, leases, occupants, payments, concerns, users, and property membership.

## Stack

- NestJS
- TypeScript
- PostgreSQL
- Prisma ORM
- Swagger / OpenAPI

## Run Application

### Prerequisites

- Node.js
- npm
- Docker Desktop or Docker Engine

### Setup

1. Install dependencies:

```bash
npm install
```

2. Create `.env` from `.env.example`.

3. Start database, apply migrations, generate Prisma client:

```bash
npm run db:setup
```

4. Start app:

```bash
npm run start:dev
```

### URLs

- API: `http://localhost:3000`
- Swagger: `http://localhost:3000/api`

## Database Setup

Docker PostgreSQL is configured in `compose.yaml`:

- host: `localhost`
- port: `5433`
- database: `nest_dev`
- user: `nest`
- password: `nest`

Commands:

```bash
npm run db:up      # start database
npm run db:down    # stop database
npm run db:reset   # remove container volume data
npm run db:setup   # start DB + migrate + generate client
```

Required env values:

```env
DATABASE_URL="postgresql://nest:nest@localhost:5433/nest_dev?schema=public"
JWT_SECRET="change-this-secret"
```

## Architecture

- **API layer:** NestJS controllers expose REST endpoints.
- **Business layer:** services contain validation and domain logic.
- **Data layer:** Prisma accesses PostgreSQL; schema lives in `prisma/schema.prisma`.
- **Auth:** JWT-based authentication with global auth guard, role guard, and property-scoped access guard.
- **Modules:** `auth`, `users`, `properties`, `units`, `occupants`, `leases`, `payments`, `concerns`, `property-members`, `prisma`.

## API Endpoints

### Auth

- `POST /auth/login`
- `GET /auth/me`

### Users

- `POST /users`
- `GET /users/me`
- `PATCH /users/me`

### Properties

- `POST /properties`
- `GET /properties`
- `GET /properties/:id`
- `PATCH /properties/:id`
- `DELETE /properties/:id`

### Units

- `POST /properties/:propertyId/units`
- `GET /properties/:propertyId/units`
- `GET /units/:id`
- `PATCH /units/:id`
- `DELETE /units/:id`

### Occupants

- `POST /occupants`
- `GET /occupants`
- `GET /occupants/:id`
- `PATCH /occupants/:id`
- `DELETE /occupants/:id`
- `GET /properties/:propertyId/occupants`
- `GET /units/:unitId/occupants`

### Leases

- `POST /leases`
- `GET /leases`
- `GET /properties/:propertyId/leases`
- `GET /leases/:id`
- `PATCH /leases/:id`
- `DELETE /leases/:id`
- `POST /leases/:id/occupants`
- `DELETE /leases/:id/occupants/:occupantId`

### Payments

- `POST /payments`
- `GET /payments`
- `GET /properties/:propertyId/payments`
- `GET /payments/:id`
- `PATCH /payments/:id`
- `DELETE /payments/:id`
- `POST /leases/:leaseId/payments`
- `GET /leases/:leaseId/payments`

### Concerns

- `POST /concerns`
- `GET /concerns`
- `GET /concerns/:id`
- `PATCH /concerns/:id`
- `DELETE /concerns/:id`
- `POST /properties/:propertyId/concerns`
- `GET /properties/:propertyId/concerns`
- `GET /units/:unitId/concerns`

### Property Members

- `POST /property-members`
- `GET /property-members`
- `GET /property-members/:id`
- `PATCH /property-members/:id`
- `DELETE /property-members/:id`
- `POST /properties/:propertyId/members`
- `GET /properties/:propertyId/members`
- `GET /users/:userId/property-members`

## Known Limitations

- No refresh token flow.
- No bootstrap/seed flow for first platform admin.
- No pagination on list endpoints.
- Some global list endpoints are still admin-only by design.
- Occupant direct detail/update/delete remains platform-admin only.
- Audit logging and activity history are not implemented.

## Improvements With More Time

- Add seed/bootstrap flow for first admin user.
- Add pagination, sorting, and richer filtering.
- Add refresh tokens and stronger auth hardening.
- Expand DB-backed e2e coverage for write flows.
- Add user administration endpoints for platform admins.
- Add audit logs and better reporting endpoints.

## Which AI Tools Were Used

- OpenAI ChatGPT through the Cave coding assistant environment
- Cave subagents for targeted codebase exploration and review support

## How AI Assisted

- Helped plan module structure, Prisma schema, and auth approach
- Refined controllers, services, DTOs, and guards
- Helped add automated tests, Docker/PostgreSQL test flow, and documentation
- Assisted with debugging lint, build, environment, and authorization issues

## Name at Least 2 AI-Generated Suggestions That You Modified or Rejected

- Suggested: Session authentication
  - Rejected due to problems with scaling and possible number of users. Modified to use JWT token auth.
- Suggested: Local application or Remote PostgresSQL server
  - Modified to use docker image of PostgresSQL to enable consistent independant work.

## How You Reviewed and Verified the Generated Output

- Jest Tests
- Thunder Client testing of endpoints
- Manual verification
