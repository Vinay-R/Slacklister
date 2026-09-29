# Slacklister

Create Spotify playlists from music links shared in your Slack channels. Connect your Slack workspace, pick which channels to watch, and SlackLister will build and maintain a Spotify playlist for each one.

<img width="1290" height="867" alt="image" src="https://github.com/user-attachments/assets/1902285c-d851-4664-a4ff-3a8940da65df" />
<img width="2580" height="1734" alt="Screenshot 2026-09-29 at 1 05 00 PM" src="https://github.com/user-attachments/assets/3bc6102e-76ce-4406-9785-94d2aaf2df39" />
<img width="2580" height="1734" alt="Screenshot 2026-09-29 at 1 05 05 PM" src="https://github.com/user-attachments/assets/1f1a2a37-de0f-45dd-aa77-870667186774" />
<img width="2580" height="1734" alt="Screenshot 2026-09-29 at 1 05 10 PM" src="https://github.com/user-attachments/assets/5c89795f-7842-42d9-8966-ae0111834f8d" />
<img width="2580" height="1734" alt="Screenshot 2026-09-29 at 1 05 15 PM" src="https://github.com/user-attachments/assets/01e6670d-1c57-458d-8091-6d47b3f3409a" />


## Features

- **Slack OAuth** -- connect any workspace in one click
- **Channel picker** -- search/filter and multi-select channels
- **Backfill scan** -- reads the full history of a channel and extracts every Spotify track & album link
- **Incremental sync** -- re-scan only new messages since the last sync
- **Spotify playlist creation** -- one playlist per channel, named `#channel-name`
- **Duplicate prevention** -- tracks are never added twice

## Prerequisites

You need two developer apps before running:

### 1. Create a Slack App

1. Go to <https://api.slack.com/apps> and click **Create New App > From scratch**
2. Name it (e.g. "Slacklister") and select your workspace
3. Under **OAuth & Permissions > Scopes > Bot Token Scopes**, add:
   - `channels:read`
   - `channels:history`
4. Under **OAuth & Permissions > Redirect URLs**, add:
   ```
   http://localhost:3000/api/auth/slack/callback
   ```
5. Copy the **Client ID** and **Client Secret** from **Basic Information**

### 2. Create a Spotify App

1. Go to <https://developer.spotify.com/dashboard> and create a new app
2. Set the **Redirect URI** to:
   ```
   http://localhost:3000/api/auth/spotify/callback
   ```
3. Check **Web API** under the APIs section
4. Copy the **Client ID** and **Client Secret**

## Getting Started

```bash
# Install dependencies
npm install

# Copy and fill in your credentials
cp .env.example .env
# Edit .env with your Slack and Spotify credentials

# Create the database
npx prisma migrate dev

# Start the dev server
npm run dev
```

Open <http://localhost:3000> and follow the on-screen steps:

1. **Connect** your Slack workspace and Spotify account on the `/connect` page
2. **Select channels** on the `/channels` page
3. **Create playlists** -- the app scans each channel and creates a Spotify playlist
4. **Sync** anytime from the `/playlists` page to pull in new tracks

## Tech Stack

- **Next.js** (App Router) + React
- **Tailwind CSS** + shadcn/ui
- **Prisma** + SQLite
- **Slack Web API** (`@slack/web-api`)
- **Spotify Web API** (direct fetch)

## Project Structure

```
src/
  app/
    page.tsx              Dashboard
    connect/page.tsx      OAuth connections
    channels/page.tsx     Channel selector
    playlists/page.tsx    Playlist manager
    api/
      auth/slack/         Slack OAuth flow
      auth/spotify/       Spotify OAuth flow
      channels/           List Slack channels
      playlists/          List tracked playlists
      scan/               Backfill scan
      sync/               Incremental sync
      status/             Connection status
  lib/
    prisma.ts             Database client
    slack.ts              Slack API helpers
    spotify.ts            Spotify API helpers
    url-parser.ts         Spotify URL regex extraction
  components/             UI components
prisma/
  schema.prisma           Database schema
```
