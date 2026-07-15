# E-Fighter

A browser-based real-time multiplayer webcam fighting game. Two players use their webcams to physically fight — punches, kicks, and blocks are detected via computer vision and translated into in-game damage. Special abilities, hand-sign gestures, and anime-style ultimate attacks add strategic depth.

## Tech Stack

- **Frontend:** Next.js + TypeScript + Tailwind CSS
- **Backend:** Python + FastAPI + WebSockets
- **Computer Vision:** MediaPipe (Pose + Hands, client-side)
- **Video:** WebRTC (peer-to-peer)
- **Voice:** Web Speech API
- **Database:** Supabase (Postgres)
- **Hosting:** Vercel + Render

## Running Locally

**Frontend**
cd frontend && npm install && npm run dev

**Backend**
cd backend && source venv/bin/activate && uvicorn main:app --reload