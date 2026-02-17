# Claruss

**Screen time enforcement for iOS — with real consequences.**

Claruss is a fullstack iOS app that enforces self-imposed screen time limits through OTP-verified unlocks and optional accountability partners. Unlike passive screen time trackers, Claruss makes it intentionally difficult to override restrictions.

## How It Works

1. **Create rules** — Pick the apps you want blocked and set a schedule (e.g., social media blocked 9am–5pm on weekdays).
2. **Rules activate automatically** — When a rule fires, selected apps are blocked at the system level via Apple's Screen Time API.
3. **Unlock requires verification** — Need access early? Enter a one-time code sent to your email. No easy dismiss button.
4. **Add an accountability partner** — Optionally require a second person to approve your unlock request.
5. **Hardcore Mode** — Enables a cooldown period before you can edit or delete rules, preventing impulsive changes.

## Features

- Schedule-based app blocking with Apple Screen Time (FamilyControls) integration
- OTP-verified early unlock via email
- Accountability partner system with invite/approve flow
- Hardcore Mode with rule-change cooldown
- Activity dashboard with lock history and event log
- Push notifications for lock events and partner requests
- Offline support with background sync
- Full audit trail of every lock, unlock, and rule change

## Tech Stack

### iOS Client (`claruss/`)
- SwiftUI, iOS 26.2, Xcode 26.2
- MVVM with `@Observable` (Observation framework)
- Swift 6 strict concurrency
- FamilyControls / Screen Time API
- URLSession async/await networking

### Backend (`server/`)
- Node.js 20 + TypeScript
- Fastify 5
- PostgreSQL 16 (via Prisma ORM)
- Redis 7 (ioredis)
- BullMQ for job processing
- JWT authentication with refresh token rotation

## Getting Started

### Backend

```bash
cd server

# Start Postgres (port 5433) and Redis
docker compose up -d

# Install dependencies
npm install

# Run migrations and generate Prisma client
npx prisma generate && npx prisma migrate dev

# Start dev server on port 3000
npm run dev
```

### iOS

Open `claruss.xcodeproj` in Xcode 26.2 and run on a simulator or device. The app connects to `http://localhost:3000` in debug builds.

```bash
# Or build from the command line
xcodebuild -project claruss.xcodeproj -scheme claruss -sdk iphonesimulator \
  -destination 'platform=iOS Simulator,name=iPhone 17 Pro' build
```

## Project Structure

```
claruss/              # iOS app source
  Models/             # Codable API response structs
  Views/              # SwiftUI views (tabs, onboarding, auth)
  ViewModels/         # @Observable view models
  Services/           # APIClient, AuthManager, ScreenTimeManager, etc.
  Extensions/         # Keychain helper, utilities
server/               # Backend API
  src/routes/         # Fastify route handlers
  src/lib/            # Auth, schemas, email, push notifications
  src/jobs/           # BullMQ worker and cron scheduler
  src/plugins/        # Prisma and Redis Fastify plugins
  prisma/             # Database schema and migrations
appstore/             # App Store submission metadata
```

## License

All rights reserved.
