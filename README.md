## Social API (Node/Express + Prisma)

Backend for the Social Mobile app. Provides user auth, posts, comments, and likes.

- **Mobile repo**: `../social-mobile`
- **DB**: SQLite (dev) via Prisma
- **Auth**: JWT Bearer in `Authorization` header

### Requirements
- Node 18+
- pnpm/npm/yarn

### Environment
Create a `.env` (if not present) with at least:
```
PORT=8800
JWT_SECRET=replace-with-strong-secret
DATABASE_URL="file:./dev.db"
```

### Install and Run
```bash
npm install
npm run dev
```
This starts the server on `http://localhost:8800`.

### Database
- Schema: `prisma/schema.prisma`
- Migrations live under `prisma/migrations/`
- Dev database: `prisma/dev.db` (SQLite)
```bash
npx prisma migrate dev
npx prisma studio
```

### Endpoints (high-level)
- `POST /users/register` – create an account
- `POST /users/login` – returns `{ token, user }`
- `GET /users/verify` – returns the current user (requires `Authorization: Bearer <token>`)
- `GET /posts` / `POST /posts` / `GET /posts/:id`
- `POST /posts/:id/like` / `DELETE /posts/:id/like`
- `GET /posts/:id/comments` / `POST /posts/:id/comments`

See `routes/` for details.

### Auth Flow
1) Client logs in at `POST /users/login`, stores `token`.
2) Client includes `Authorization: Bearer <token>` on protected routes.
3) Mobile app auto-verifies token on startup via `GET /users/verify`.

### Project Structure (api)
```
index.js              # Express app entry
middlewares/auth.js   # JWT verification
routes/
  user.js
  post.js
  comment.js
prisma/
  schema.prisma
  migrations/
```

### CORS
If you access from device/simulator, ensure CORS is configured to allow the app origin or use permissive defaults in development.

### Seeding
```bash
node prisma/seeds/main.js
```

### Related
- Mobile app: see `../social-mobile/README.md`


