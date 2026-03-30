# uhm
uhm is a social platform where the primary unit of expression is audio. Users record short voice posts called Waves — up to 60 seconds — and share them to a feed, explore trending topics by sound, and connect through voice DMs. Text is secondary. Your voice is the content.

What's been designed so far
Five core screens have been fully designed and prototyped:

Home Feed — chronological/algorithmic feed of Waves with waveform players and social actions
Record Screen — rich, animated recording UI with a 60-second ring timer and pill category tagging
Explore / Discovery — search, category browsing, algorithmic "Right Now" hero, and trending topics
User Profile — audio portfolio layout with a voice intro, pinned Wave, and listener stats
Notifications — grouped today / chronological older, with Wave snippets inline
Messages — voice + text DMs, Wave sharing, emoji reactions, read receipts, and message requests


Core concepts
Wave — a single voice post, max 60 seconds.
Pill — a category tag the creator picks before recording. Sets listener expectations and powers discovery. Current options: Hear Me Out, Hot Take, Storytime, Question, Vibe Check, Rant, Announcement.
Drop a Wave — the act of posting. No drafts, no review. You record, you drop.

Tech stack
Frontend (MVP — web app)

React — component-based UI
Tailwind CSS — utility-first styling
Web Audio API — waveform visualization and audio recording in-browser

Backend

Node.js — API server
Express — routing and middleware
PostgreSQL — primary database (users, posts, follows, likes)
S3-compatible storage — audio file hosting (e.g. AWS S3 or Cloudflare R2)
WebSockets — live notifications and real-time feed updates

Planned (post-MVP)

client (React)
    │
    ▼
API server (Node / Express)
    │
    ├── Auth service (JWT)
    ├── Feed service (posts, timeline)
    ├── Audio service (upload → S3, transcode)
    ├── Notification service (WebSocket)
    └── Search service (tag + user lookup)
    │
    ▼
PostgreSQL + S3

Audio flow: browser records via Web Audio API → uploads raw blob to API → API stores to S3 and saves metadata to Postgres → feed renders waveform from audio URL.

Design decisions & principles
Voice is the hero, text is the footnote. Every screen is built around the audio content. Text (captions, bios) supports the voice — it never competes with it.
60-second limit is a feature. Forces creators to be concise and makes lurkers more likely to listen all the way through. A Wave you finish builds trust with the algorithm.
Pills shape culture, not just feeds. The pill taxonomy (Hot Take, Storytime, etc.) becomes the vernacular of the platform. People won't say "I posted a Wave" — they'll say "I dropped a Hot Take." The labels become social shorthand.
Instant post, no review. The record button is a commitment. No preview screen means the platform rewards authenticity over polish. You can re-record before posting, but once you drop it, it's live.
Listeners > followers as the key metric. A follower is passive. A listener heard you. Listener counts are surfaced prominently on profiles and the explore page to reward actual engagement.
Waveform cover on profiles. Instead of a generic banner photo, every profile has a generated waveform pattern. Every profile is literally made of sound.

waveform/
├── client/                  # React frontend
│   ├── src/
│   │   ├── components/      # Reusable UI components
│   │   │   ├── feed/        # Feed, WaveCard, WavePlayer
│   │   │   ├── record/      # RecordScreen, RingTimer, LiveBars
│   │   │   ├── explore/     # SearchBar, TrendingGrid, HeroCard
│   │   │   ├── profile/     # ProfileHeader, WaveList, VoiceIntro
│   │   │   ├── notifications/
│   │   │   └── messages/    # Inbox, ConvoThread, VoiceMessage
│   │   ├── pages/           # Top-level route pages
│   │   ├── hooks/           # useAudioRecorder, useWaveform, usePlayer
│   │   ├── lib/             # API client, auth helpers
│   │   └── styles/          # Tailwind config, global styles
│   └── public/
│
├── server/                  # Node.js backend
│   ├── routes/              # Express route handlers
│   ├── controllers/         # Business logic
│   ├── models/              # DB models / queries
│   ├── middleware/          # Auth, rate limiting, upload
│   ├── services/            # S3, transcription, notifications
│   └── utils/
│
├── design/                  # Design assets and prototypes
│   └── screens/             # HTML prototypes of all 6 screens
│
└── docs/                    # Additional documentation

Getting started

⚠️ This is a solo MVP in early development. Things will move fast and break.

Prerequisites

Node.js 18+
PostgreSQL 15+
An S3-compatible bucket (AWS S3 or Cloudflare R2)

# Clone the repo
git clone https://github.com/yourusername/waveform.git
cd waveform

# Install dependencies
npm install --workspaces

# Set up environment variables
cp .env.example .env
# Fill in your DB credentials, S3 keys, and JWT secret

# Run database migrations
npm run db:migrate

# Start the dev server (runs client + server concurrently)
npm run dev

Client runs on http://localhost:3000, API on http://localhost:4000.

Environment variables

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/waveform

# Auth
JWT_SECRET=your_jwt_secret_here

# Storage
S3_BUCKET=your-bucket-name
S3_REGION=us-east-1
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret

# Optional: Audio transcription
OPENAI_API_KEY=your_key

Roadmap
MVP (v0.1)

 Auth — sign up, log in, JWT sessions
 Record and upload a Wave
 Home feed with audio playback
 Like, repost, follow
 User profiles
 Basic explore / search

v0.2

 Pill category system
 Notifications (likes, reposts, new followers)
 Voice + text DMs
 Wave sharing in DMs
 Message requests

v0.3

 Algorithmic feed ranking
 Trending topics by pill category
 Listener count tracking
 Auto-transcription for accessibility

Post-MVP

 React Native mobile app
 Wave threads (chained audio posts)
 Audio replies to Waves
 Creator analytics dashboard
 Milestone notifications (1k listeners, etc.)


Contributing
This is a personal project for now, but if you've stumbled in and want to help — welcome.
A few things before you open a PR:

Check the roadmap first. If it's not on the list, open an issue to discuss it before building.
Keep components small. One job per component. If it's doing three things, split it.
Voice is the product. Any feature that buries audio under text is probably the wrong direction.
No design changes without a prototype. Use the existing screen prototypes in /design/screens/ as reference. Don't change the visual language without discussing it first.

Bug reports and issues are always welcome — just be specific about the device, browser, and what you expected to happen.

License
MIT — do what you want, just don't pass it off as your own product.

Built with too much enthusiasm and just enough caffeine.
React Native app (iOS + Android)
Audio transcription via Whisper API for accessibility and search indexing
CDN for audio delivery with low-latency streaming
