# AI DETECTION

AI DETECTION is a real-time online exam monitoring console for reviewing a candidate's camera feed, session signals, evidence events, and provider health.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/ai-detection/src/pages/monitoring-workspace.tsx` — live monitoring workspace, webcam access, recording, source selection, signal presentation, and session controls
- `artifacts/ai-detection/src/index.css` — visual language and monitoring-console theme
- `artifacts/api-server/src/routes/monitoring.ts` — monitoring overview, events, session lifecycle, and provider status API
- `lib/api-spec/openapi.yaml` — source of truth for monitoring API contracts
- `lib/api-client-react/src/generated/` and `lib/api-zod/src/generated/` — generated client hooks and validation schemas

## Architecture decisions

- Webcam access and MediaRecorder stay in the browser; recordings download locally as WebM rather than uploading candidate video by default.
- The API keeps the initial session and event state in memory so candidate media is not persisted implicitly.
- Gravity AI provider state is explicit and currently reports `standby`; the UI does not claim live provider results until a provider connection is available.
- API contracts are generated from OpenAPI and consumed through the shared React Query client.

## Product

- Candidate profile display for Guru, age 24, semester 7th
- Browser and mobile capture-source profiles
- Live webcam permission state and pause/resume controls
- Local video recording download
- Confidence gauge with face, gaze/head-turn, extra-person, and restricted-object signal rows
- Evidence timeline, API health, session start/end, and provider connection status

## User preferences

- Use the product name “AI DETECTION”.
- Keep the monitoring experience direct, serious, and real-time.

## Gotchas

- Camera and recording require browser permissions and a browser that supports `getUserMedia` and `MediaRecorder`.
- Gravity AI is intentionally transparent about connection state; do not turn standby into connected without adding a real provider integration.
- Run OpenAPI codegen after changing `lib/api-spec/openapi.yaml`.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
