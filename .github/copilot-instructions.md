# Copilot Instructions for UIGen

## 1. Big picture architecture (essential for fast onboarding)
- App is a Next.js 15 App Router SPA in `src/app`.
- `src/app/main-content.tsx` wires together `FileSystemProvider` + `ChatProvider` + UI split panels (chat left, preview/code right).
- AI pipeline runs through API endpoint `src/app/api/chat/route.ts`.
  - Accepts `messages`, `files`, `projectId`.
  - Rehydrates `VirtualFileSystem` from nodes.
  - Uses `@ai-sdk/anthropic` or mock model to stream text and tool calls.
  - Saves project state to Prisma in `onFinish` (messages + serialized filesystem data).

## 2. Core data flow and service boundaries
- Virtual file system: `src/lib/file-system.ts` (`createFile`, `updateFile`, `deleteFile`, `rename`, `serialize`, `deserialize`).
- Tool adapters:
  - `src/lib/tools/str-replace.ts` => `str_replace_editor` (create/str_replace/insert/view)
  - `src/lib/tools/file-manager.ts` => `file_manager` (rename/delete)
- Client state: `src/lib/contexts/file-system-context.tsx` and `src/lib/contexts/chat-context.tsx`.
- Project persistence & auth server actions:`src/actions/*.ts` (`getProject`, `getProjects`, `createProject`) use `getSession()` + Prisma.

## 3. Model selection and request behavior
- `src/lib/provider.ts` chooses either:
  - `anthropic` client with `MODEL="claude-haiku-4-5"`
  - `MockLanguageModel` when `ANTHROPIC_API_KEY` is missing.
- In `src/app/api/chat/route.ts`, `maxSteps` is `4` for mock and `40` for real API.

## 4. Project conventions (non-generic, source-specific)
- React components are client-only in `src/components/*` and follow Tailwind + Radix UI primitives.
- actions under `src/actions/` are `"use server"`; UI components are `"use client"`.
- `FileTree` + `CodeEditor` in `src/components/editor/` operate on `VirtualFileSystem` abstract state, not disk.
- `PreviewFrame` in `src/components/preview/PreviewFrame.tsx` picks `/App.jsx` or first JSX/TSX entry point and uses `src/lib/transform/jsx-transformer.ts`.
- Auth is cookie-JWT based in `src/lib/auth.ts` (`auth-token`, 7-day expiry). 

## 5. Essential commands and workflows
- `npm run setup` => `npm install && npx prisma generate && npx prisma migrate dev`.
- `npm run dev` => start local dev with turbopack using `node-compat.cjs` shim.
- `npm run build`, `npm run start` for prod.
- `npm run test` (vitest), `npm run lint` (next lint).
- `npm run db:reset` resets Prisma DB.

## 6. Debugging and file-based hotspots
- If chat tool-call transcript breaks, inspect `src/lib/contexts/chat-context.tsx` for `onToolCall` + file ops.
- For persistence issues, inspect `src/app/api/chat/route.ts` (`onFinish` writes `project` with `JSON.stringify(fileSystem.serialize())`).
- For model prompt behavior, inspect `src/lib/prompts/generation.ts` and `src/lib/provider.ts`.

## 7. Merge/update guidance
- Add any future instructive decisions into this file; prioritize project-specific source paths.
- Keep phrases like "do not write files to disk"; this repo uses in-memory virtual FS sync via context + API.

> Feedback requested: Are any sections unclear or missing (e.g., tokenizer limits, error paths, non-AI endpoints)?
