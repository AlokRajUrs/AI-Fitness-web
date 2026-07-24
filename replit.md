# AI Fitness Academy

An AI-powered fitness SaaS platform with a premium dark futuristic UI — workout tracking, diet planning, AI chat coach, progress analytics, achievements, and an admin dashboard.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/ai-fitness-academy run dev` — run the frontend (port 23615)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, Framer Motion, Recharts, Wouter, shadcn/ui
- API: Express 5 (artifacts/api-server)
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)

## Where things live

- `lib/api-spec/openapi.yaml` — Single source of truth for all API contracts
- `lib/db/src/schema/` — Drizzle table definitions (users, workouts, meals, diet_plans, progress_entries, measurements, chat_messages, achievements)
- `artifacts/api-server/src/routes/` — Express route handlers (profile, dashboard, workouts, diet, progress, chat, achievements, admin)
- `artifacts/ai-fitness-academy/src/` — React frontend (pages: landing, dashboard, workouts, diet, progress, chat, achievements, admin)
- `lib/api-client-react/src/generated/` — Generated React Query hooks (do not edit)
- `lib/api-zod/src/generated/` — Generated Zod schemas for server validation (do not edit)

## Architecture decisions

- Demo user (id=1) is used for all data — no auth in the first build; all endpoints operate on userId=1.
- Diet calculations use Harris-Benedict BMR formula with activity multipliers.
- AI chat responses are pre-scripted fitness coaching messages (no external LLM required for MVP).
- Dashboard stats blend real workout/progress DB data with sensible defaults for metrics not yet tracked (steps, sleep, heart rate).

## Product

- **Landing page** — hero, stats, 7 AI module cards, testimonials, pricing (Starter/Pro/Elite), FAQ, footer
- **Dashboard** — real-time stat widgets, weekly analytics chart, recent activity feed
- **Workouts** — log sessions, view history, workout summary stats
- **Diet Planner** — BMI/calorie calculator, macro breakdown, meal plan (breakfast/lunch/dinner/snacks)
- **Progress Tracker** — weight/body fat trend charts, body measurements, progress log
- **AI Gym Buddy** — conversational chat interface with AI coaching responses
- **Achievements** — badge wall with earned/locked states and progress bars
- **Admin Dashboard** — platform analytics, user table, revenue chart by month

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- After any OpenAPI spec change, run `pnpm --filter @workspace/api-spec run codegen` before touching frontend or server code.
- Array columns in Drizzle: use `.array()` method — `text("tags").array()`, not `array(text(...))`.
- Express 5 wildcard routes need names: `/{*splat}` not `*`.
- `req.params.id` is `string | string[]` in Express 5 — always parse: `parseInt(Array.isArray(p) ? p[0] : p, 10)`.
