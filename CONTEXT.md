# E-Fighter — Project Context

## What This Is

E-Fighter is a browser-based, real-time multiplayer fighting game where two players use their webcams to physically "fight" each other. Body movements (punches, kicks, blocks, dodges) are detected via computer vision and translated into in-game actions. Players equip a loadout of special abilities triggered by hand signs or voice commands, charge an ultimate meter over the course of a match, and battle until one player's health bar is depleted.

The concept builds on viral "e-fighting" videos and Omegle-style random pairing, but differentiates from existing tools (e.g. Shadowbox) through a broader move set, an anime-style ability/ultimate system, and gesture/voice-triggered powers.

## Tech Stack

- **Frontend:** Next.js + TypeScript
- **Backend:** Python + FastAPI (WebSockets for game state sync, REST for auth/leaderboard)
- **Computer Vision:** MediaPipe (Pose model for body tracking, Hands model for gesture detection) — runs fully client-side in the browser
- **Video:** WebRTC peer-to-peer (webcam streams go directly between players, never through the server)
- **Voice:** Web Speech API (browser-native, no external API)
- **Effects:** Canvas 2D API (hit effects, ability/ultimate overlays)
- **Database:** Supabase (auth, users, match history, ELO)
- **Hosting:** Vercel (frontend), Render (backend)

> Note: No external/paid APIs. MediaPipe, WebRTC, Web Speech, and Canvas are all browser-native or free open-source libraries.

## Core Features (Locked Scope)

1. **Body tracking + directional hit accuracy** — 33-landmark pose tracking, dynamic per-frame hitboxes (convex hull of torso/head landmarks), trajectory-based hit detection so aim matters.
2. **Move set** — jab, cross, hook, uppercut, kick, block, dodge. Each detected via landmark velocity thresholds.
3. **Ability system** — 3 loadout slots chosen pre-match from a roster of ~10 abilities (shield, lightning strike, heal, power surge, blind, stun, rage, counter, speed boost, drain). Each has an individual cooldown. Triggered by hand signs OR voice commands.
4. **Ultimate bar** — fills on hits dealt/taken; once full, triggers a powerful ultimate (meteor strike, time stop, full heal, berserker, death mark, void slash).
5. **Voice commands** — opt-in, Web Speech API, mappable to ability slots.
6. **Visual effects** — Canvas-rendered hit effects, per-ability overlays, per-ultimate cinematics, KO sequence.
7. **Matchmaking** — private rooms (shareable link) + random queue.
8. **Accounts + ELO + leaderboard** — Supabase auth (guest play allowed), ELO updated per match, rank tiers (Bronze → Silver → Gold → Challenger), match history, profile.

## Screen Flow

Landing → Matchmaking/Room → Loadout → Fight → Results → (Rematch or back to Matchmaking)

## Build Phases

- **Phase 0 — Setup:** Next.js + TS repo, FastAPI backend with health check, Supabase connected, deploy shells to Vercel + Render, basic landing + placeholder fight screen.
- **Phase 1 — Webcam + Peer Connection:** WebRTC P2P, room system (private + random queue via WebSocket), ready check, basic fight layout.
- **Phase 2 — Body Tracking + Move Detection:** MediaPipe Pose + Hands, velocity-based move detection, dynamic hitbox, directional accuracy check, threshold tuning.
- **Phase 3 — Core Game Loop:** synced health bars, damage values, round timer, KO/win-loss, hit flash, lag compensation.
- **Phase 4 — Ability System:** loadout screen, hand sign detection, all abilities + logic, cooldowns, activation window, ultimate bar + ultimates.
- **Phase 5 — Visual Effects + Polish:** Canvas animation system, hit/ability/ultimate effects, combo counter, KO cinematics, sound.
- **Phase 6 — Accounts + ELO + Leaderboard:** Supabase auth, ELO, match history, leaderboard, profile, results share card.
- **Phase 7 — Final Polish:** UI cleanup, responsiveness, README + demo video.

## Current Status

Starting Phase 0. Nothing built yet.

## Immediate Next Steps (Phase 0)

1. Scaffold a Next.js + TypeScript project.
2. Scaffold a FastAPI backend with a `/health` route.
3. Wire up Supabase (project + client config; sketch the schema: `users`, `matches`, `elo_history`).
4. Deploy frontend to Vercel and backend to Render to confirm the pipeline works end to end.
5. Build a placeholder landing page (Play Now / Play with Friend buttons) and an empty fight screen route.

## Key Design Notes / Gotchas

- **Phase 2 is make-or-break.** Validate MediaPipe move detection feels responsive before building on top of it. Consider spiking this in isolation first.
- Hand signs are NOT finalized — they'll be tuned after testing what MediaPipe reliably distinguishes. Keep ability → trigger mapping configurable.
- Run two MediaPipe models simultaneously (Pose + Hands); watch performance.
- Each player runs their own CV locally; only resulting game events (hits, ability activations) sync over WebSocket. Don't stream landmarks over the network.
- Lighting/occlusion degrade landmark confidence — gate move registration behind a confidence threshold, show a low-visibility warning.
- Voice recognition runs locally per client so opponents can't trigger each other's commands.
