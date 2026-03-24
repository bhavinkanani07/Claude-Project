# Claude-Project

## Overview
AI-powered React component generator with live preview.

## Prerequisites
- Node.js 18+
- npm

## Setup
1. Copy `.env` and set Anthropic key:

```bash
cp .env .env.local
ANTHROPIC_API_KEY="your-anthropic-api-key-here"
```

2. Install dependencies and DB:

```bash
npm run setup
```

3. Start development server:

```bash
npm run dev
```

4. Open app:

http://localhost:3000

## Local workflows
- Chat generator endpoint: `src/app/api/chat/route.ts`
- Virtual FS: `src/lib/file-system.ts`
- Chat context: `src/lib/contexts/chat-context.tsx`
- File provider: `src/lib/contexts/file-system-context.tsx`

## GitHub integration quickstart
1. Ensure SSH key is in GitHub: https://github.com/settings/ssh/new
2. Set remote URL:

```bash
git remote set-url origin git@github.com:bhavinkanani07/Claude-Project.git
```

3. Push updates:

```bash
git add .
git commit -m "<message>"
git push -u origin main
```

## CI
File: `.github/workflows/ci.yml` runs:
- `npm ci`
- `npm run lint`
- `npm run test` on push/pr to `main`

## Troubleshooting

### API key issues
- **"invalid x-api-key"**: Check your `ANTHROPIC_API_KEY` in `.env` is correct and not expired.
- **403 Forbidden**: Ensure your Anthropic account has credits and the key has access.
- **Mock mode fallback**: Leave `ANTHROPIC_API_KEY` empty or invalid to use static demo code.

### GitHub push issues
- **Permission denied (publickey)**: Add your SSH key to GitHub at https://github.com/settings/ssh/new
- **403 with HTTPS**: Use SSH remote or create a personal access token with `repo` scope.

### App startup
- **Port 3000 in use**: App uses 3001 automatically.
- **No files in preview**: Send a prompt like "Create a form component" in chat.
- **DB errors**: Run `npm run setup` to initialize Prisma.