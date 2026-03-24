# Claude Instructions for UIGen

## Overview
UIGen is an AI-powered React component generator built with Next.js 15, using a virtual file system and Anthropic's Claude for code generation.

## Key Architecture
- **Frontend**: Next.js App Router with client-side components in `src/components/`
- **Backend**: API route at `src/app/api/chat/route.ts` handles streaming responses
- **File System**: Virtual in-memory system (`src/lib/file-system.ts`) for code manipulation
- **Tools**: Custom tools for file operations (`src/lib/tools/str-replace.ts`, `src/lib/tools/file-manager.ts`)
- **Persistence**: Prisma with SQLite for user projects and messages

## Usage
1. Set `ANTHROPIC_API_KEY` in `.env`
2. Run `npm run setup` then `npm run dev`
3. Use the chat interface to generate React components
4. Preview components live in the right panel

## Important Notes
- Components are generated in JSX/TSX format
- Virtual file system syncs via React context
- Auth uses JWT cookies (7-day expiry)
- Mock mode activates without API key

## Development
- `npm run test` for Vitest
- `npm run lint` for ESLint
- Build with `npm run build`

> This file provides context for Claude AI interactions within the UIGen project.