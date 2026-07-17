# YouTube 2.0 — Full-Stack Clone

A YouTube-style video platform built with **React + Tailwind CSS** on the frontend and
**Node.js + Express** on the backend.

## Stack
- **Frontend:** React 18, Vite, Tailwind CSS, React Router, Axios
- **Backend:** Node.js, Express, lowdb (JSON file database — no external DB/setup needed), JWT auth, Multer (file uploads)

## Features
- 🏠 Home feed with category filters (sidebar)
- 🔍 Search videos by title/description
- ▶️ Watch page: HTML5 video player, like/dislike, subscribe, view count, description, comments
- 💬 Real comments tied to logged-in users
- 📹 Upload page: upload your own video file + thumbnail, with a title/description/category
- 👤 Channel pages showing all of a channel's videos + subscriber count
- 🔐 Auth: signup/login with JWT, passwords hashed with bcrypt
- 📱 Responsive, YouTube-style UI (red/white theme, collapsible sidebar)

## Project structure
```
youtube2.0/
├── backend/
│   ├── server.js            # Express app entrypoint
│   ├── db.js                 # lowdb setup + seed demo videos
│   ├── middleware/auth.js    # JWT auth middleware
│   ├── routes/
│   │   ├── auth.js           # signup / login / me
│   │   ├── videos.js         # list/search, get, upload, like/dislike, comments
│   │   └── channels.js       # channel profile + subscribe
│   └── uploads/              # uploaded video/thumbnail files land here
└── frontend/
    ├── vite.config.js        # dev server + proxy to backend on :5000
    ├── tailwind.config.js
    └── src/
        ├── App.jsx            # routes + layout
        ├── context/AuthContext.jsx
        ├── api/api.js         # axios instance with auth header
        ├── components/        # Navbar, Sidebar, VideoCard, CommentSection
        └── pages/              # Home, Watch, SearchResults, Channel, Upload, Login, Signup
```

## Getting started

You need **two terminals** — one for the backend, one for the frontend.

### 1. Backend (port 5000)
```bash
cd backend
npm install
npm start
```
You should see: `YouTube 2.0 backend running on http://localhost:5000`

The database is a local file (`backend/db.json`) auto-created on first run, seeded with
3 demo videos so the app isn't empty. Uploaded files go into `backend/uploads/`.

### 2. Frontend (port 5173)
```bash
cd frontend
npm install
npm run dev
```
Open **http://localhost:5173** in your browser. The Vite dev server proxies `/api` and
`/uploads` requests to the backend on port 5000, so both must be running.

### Production build
```bash
cd frontend
npm run build      # outputs static files to frontend/dist
npm run preview    # serve the production build locally to test it
```
For a real deployment, serve `frontend/dist` from any static host (Netlify, Vercel, S3+CloudFront,
or Express itself via `express.static`) and point it at your deployed backend URL (update the
proxy/base URL accordingly — see "Production notes" below).

## Test it out
1. Open the app — you'll see 3 seeded demo videos on the home page.
2. Click **Sign in → Create one** to sign up (any email/password works, min 6 char password).
3. Once signed in: like/dislike a video, leave a comment, subscribe to a channel.
4. Click **Upload** (📹 icon or sidebar) to publish your own video — pick any local video
   file, add a title, and publish. It'll appear on the home page and your channel page.
5. Use the search bar to filter by title/description; use sidebar categories to filter by genre.

## Production notes
- **Database:** `lowdb` (JSON file) is great for demos/prototypes but not for concurrent
  production traffic. Swap `db.js` for MongoDB/Postgres when you're ready to scale.
- **File storage:** Uploaded videos currently save to local disk (`backend/uploads/`). For
  production, upload to S3/Cloudinary/Cloud Storage instead so files survive server restarts
  and redeploys.
- **JWT secret:** Set a real `JWT_SECRET` environment variable in production — don't rely on
  the dev fallback in `middleware/auth.js`.
- **HTTPS:** Required in production for secure cookie/token handling.
- **Video transcoding:** This clone plays uploaded files directly; a production platform would
  transcode uploads into adaptive bitrate streams (HLS/DASH) using something like ffmpeg.
