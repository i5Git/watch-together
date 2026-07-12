# Watch

Watch is a private watch-together app for synchronized video playback, room chat, subtitles, and mobile-friendly rooms.

This is the standalone Watch repository, based on the open-source WatchParty project. The upstream project and its contributors remain credited in the repository history and [LICENSE](./LICENSE). Watch keeps the original MIT license obligations.

## What changed

- Rebranded the product UI to **Watch**.
- Rebuilt the landing page and room workspace with a maintainable dark cinema design system.
- Added Persian-first RTL behavior for navigation, room controls, chat, forms, menus, and responsive mobile layouts.
- Removed profile photos and avatar rendering from visible room and account flows.
- Removed visible GitHub and Discord credit links from the product UI.
- Preserved synchronized rooms, direct MP4/HLS playback, chat, playlists, subtitles, screen sharing, file sharing, and optional virtual-browser support.
- Removed upstream client credential defaults and hard-coded TURN credentials. Configure those values through `.env`.
- Added an Ubuntu installer with Docker Compose by default and an optional native systemd path.

## Quick start on Ubuntu

The easiest deployment path downloads the installer, clones Watch into `/opt/watch`, installs Docker Compose, starts the service, and restarts it after reboot:

```bash
curl -fsSL https://raw.githubusercontent.com/i5Git/watch-together/main/scripts/install-ubuntu.sh | bash -s -- --yes
```

The installer defaults to port `8080` and Docker Compose. It prints the final URL and status/log commands when it finishes.

For an interactive setup:

```bash
curl -fsSL https://raw.githubusercontent.com/i5Git/watch-together/main/scripts/install-ubuntu.sh | bash
```

Useful options:

```bash
# Update the existing /opt/watch checkout and rebuild it.
curl -fsSL https://raw.githubusercontent.com/i5Git/watch-together/main/scripts/install-ubuntu.sh | bash -s -- --update --yes

# Use another directory or public port.
curl -fsSL https://raw.githubusercontent.com/i5Git/watch-together/main/scripts/install-ubuntu.sh | \
  bash -s -- --yes --dir /srv/watch --port 8080

# Prompt for optional Firebase, Stripe, TURN, Postgres, and Redis configuration.
curl -fsSL https://raw.githubusercontent.com/i5Git/watch-together/main/scripts/install-ubuntu.sh | \
  bash -s -- --advanced
```

If you already cloned the repository:

```bash
chmod +x scripts/install-ubuntu.sh
./scripts/install-ubuntu.sh --yes
```

The updater pulls the `main` branch and rebuilds the Compose service without overwriting an existing `.env`.

## Manual Docker deployment

```bash
cp .env.example .env
# Edit .env locally. Do not commit it.
docker compose up -d --build
docker compose ps
```

The application listens inside the container on port `8080`. Set `APP_PORT` in `.env` to choose the host port.

## Local development

Requires Node.js 24 or newer.

```bash
npm ci
npm run ui
```

In another terminal:

```bash
npm start
```

The Vite UI runs on its development port and the Node server defaults to port `8080`.

## Configuration

All configuration is optional unless you need authentication, permanent rooms, subscriptions, external media search, Redis, Postgres, TURN, or virtual-browser management.

- Client-side build variables are documented in `.env.example`.
- Server-side variables and defaults are documented in `server/config.ts`.
- Never commit `.env`, Firebase Admin JSON, Stripe secret keys, database URLs, Redis URLs, or TURN credentials.

For browser-compatible direct media playback, use MP4 files with H.264/AAC or HLS streams that support HTTP range requests. MKV compatibility depends on the browser and codecs inside the container.

## License

Watch remains distributed under the MIT License. See [LICENSE](./LICENSE) for the full notice.
