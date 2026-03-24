# CorperCompass Frontend 🧭

> Single-file vanilla HTML/CSS/JS frontend for the CorperCompass NYSC relocation platform.

---

## Overview

This is the complete frontend for CorperCompass — a single self-contained HTML file that connects to the CorperCompass backend API. No build tools, no frameworks, no dependencies to install. Open the file in a browser and it works.

---

## Quick Start

1. Download `corpercompass-app.html`
2. Open it in any modern browser — or serve it with any static file server
3. Change the API base URL at the top of the `<script>` section (see [Configuration](#configuration))

That's it.

---

## Configuration

At the top of the `<script>` block inside `corpercompass-app.html`, you'll find:

```js
const CONFIG = {
  BASE_URL: 'http://localhost:5000/api',
};
```

Change `BASE_URL` to point to wherever the backend is running:

| Environment | Value |
|---|---|
| Local development | `http://localhost:5000/api` |
| Railway | `https://your-app.railway.app/api` |
| Render | `https://your-app.onrender.com/api` |
| Custom domain | `https://api.yourdomain.com/api` |

This is the **only line you need to change** between environments.

---

## Features

All pages are fully wired to the backend API:

| Page | Route(s) used |
|---|---|
| Login / Register | `POST /auth/login`, `POST /auth/register` |
| Dashboard | `GET /auth/me`, `GET /checklist/journey`, `GET /checklist/progress` |
| Areas | `GET /areas`, `GET /areas/:id` |
| Housing | `GET /lodges`, `GET /lodges/:id` |
| Checklist | `GET /checklist/journey`, `GET /checklist/progress`, `PATCH /checklist/:itemId` |
| Budget | `POST /budget` |
| Culture Guide | `GET /culture`, `GET /culture/:id` |
| Map | `GET /map/markers` |
| Messages | `GET /messages`, `POST /messages`, `GET /messages/conversation/:userId` |
| Profile | `GET /users/profile`, `PUT /users/profile` |

---

## Authentication

The app stores the JWT returned from login/register in `localStorage` under the key `cc_token`. On page load it checks for a stored token and auto-logs in if one exists. Logging out clears it.

The backend uses HTTP-only cookies but also returns the token in the response body — the frontend uses the Bearer token approach. Make sure your backend includes the token in the login/register response body like:

```json
{
  "token": "eyJhbGc...",
  "user": { ... }
}
```

---

## CORS

For the frontend to communicate with the backend from a different origin, the backend must allow the frontend's origin. In your backend `.env`:

```env
FRONTEND_URL=http://localhost:5500
```

If deploying to a custom domain or Vercel, update `FRONTEND_URL` to match.

---

## Serving the File

Any static file server works:

```bash
# Python (quickest for local testing)
python3 -m http.server 5500

# Node (if you have live-server installed)
npx live-server --port=5500

# VS Code
# Install the "Live Server" extension, right-click the file → Open with Live Server
```

Then visit `http://localhost:5500/corpercompass-app.html`.

---

## File Structure

There is one file:

```
corpercompass-app.html   ← everything lives here
```

Internally it's organized as:

```
<style>          CSS design system + all page styles
<body>           Auth pages (login, register)
                 App shell (sidebar, topbar)
                 All app pages (dashboard, map, areas, etc.)
                 Modals
<script>         CONFIG object (change BASE_URL here)
                 State management
                 API utility functions
                 Auth logic
                 Page navigation
                 Data loading + rendering per page
                 Helper utilities
                 Boot sequence
```

---

## Design

- Fonts: [Syne](https://fonts.google.com/specimen/Syne) (headings) + [DM Sans](https://fonts.google.com/specimen/DM+Sans) (body) — loaded from Google Fonts
- Map: [Leaflet.js](https://leafletjs.com/) v1.9.4 — loaded from CDN
- No other external dependencies

---

## Backend Repo

See the backend repository for API documentation, environment setup, and deployment instructions:

> [CorperCompass Backend →](https://github.com/copper-compass/CorperCompass)

Backend base URL pattern: `/api/*`  
Full API reference: see `README.md` in the backend repo

---

## Browser Support

Any modern browser (Chrome, Firefox, Safari, Edge). Does not require any polyfills.

---

## License

MIT
