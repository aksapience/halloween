# Monster Mash Trivia

A live, play-along Halloween trivia game you host from one screen while everyone joins
from any device with a room code — no accounts, no sign-in for players. Auto-scored with
speed bonuses, a live leaderboard, and two double-point "Final Round" questions.

Built to run as a free static site (GitHub Pages) with a free realtime backend (Firebase).
Styled in the Sapience AI brand.

---

## What you need (both free, ~15–20 min, one time)

1. A **GitHub** account — to host the page.
2. A **Firebase** account (uses your Google login) — for the live scoring.

You only do this setup once. After that, hosting a game is one click.

---

## Step 1 — Put the game on GitHub Pages

1. Create a new repository at <https://github.com/new> (e.g. `monster-mash-trivia`). Make it **Public**.
2. Upload `index.html` and `README.md` (the repo page has an **"uploading an existing file"** link — drag them in, or use **Add file → Upload files**). Commit.
3. In the repo, go to **Settings → Pages**.
4. Under **Build and deployment → Source**, pick **Deploy from a branch**, choose branch **main** and folder **/ (root)**, and **Save**.
5. Wait ~1 minute. Your game is live at:
   `https://YOUR-USERNAME.github.io/monster-mash-trivia/`

Open that URL — you'll see a **"One-time setup"** screen until you finish Step 2.

---

## Step 2 — Create the Firebase backend

1. Go to <https://console.firebase.google.com> → **Add project**. Name it anything
   (e.g. `monster-mash`). You can **disable Google Analytics** — not needed. Create.
2. In the left menu: **Build → Realtime Database → Create Database**.
   - Pick a location, then choose **Start in test mode** → Enable.
   - (Test mode is fine; we tighten the rules in Step 4.)
3. Add a web app so you get config keys: click the **⚙ gear → Project settings →**
   scroll to **Your apps → Web (`</>`)**. Register an app (any nickname). Firebase shows a
   `firebaseConfig = { … }` snippet — **keep that tab open**, you need it next.

---

## Step 3 — Paste your config into the game

1. Open `index.html` (edit it right in GitHub: open the file → pencil **✏ Edit**).
2. Near the top, find the block that starts with `window.FIREBASE_CONFIG`. Replace the
   `PASTE_…` placeholders with the values from your Firebase snippet:

   ```js
   window.FIREBASE_CONFIG = {
     apiKey: "AIza…",
     authDomain: "monster-mash.firebaseapp.com",
     databaseURL: "https://monster-mash-default-rtdb.firebaseio.com",
     projectId: "monster-mash",
     appId: "1:…:web:…"
   };
   ```

   > Make sure `databaseURL` is included — Firebase's snippet sometimes omits it. You'll
   > find it on the **Realtime Database** page (it's the URL at the top, ending in
   > `.firebaseio.com`).

3. Commit the change. Reload your game URL — the setup screen is gone and you'll see
   **Host a game / Join a game**.

---

## Step 4 — Set the database rules

By default test mode locks itself after 30 days. Replace the rules so the game keeps
working and only touches its own game rooms.

In **Realtime Database → Rules**, paste this and **Publish**:

```json
{
  "rules": {
    "rooms": {
      "$code": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

This lets anyone with a room code read and write that one room — which is all a party game
needs. Rooms are throwaway; delete old ones anytime with the **End room** button, or in the
Firebase console.

---

## How to run it on game day

1. Open your game URL on the screen you'll share → **Host a game**. You get a **room code**.
2. Share the game URL + the code with players (chat, email, calendar invite). They open the
   URL → **Join a game** → enter the code and a name. They appear in your lobby.
3. Pick seconds per question, hit **Start**. Each question: players tap an answer, you watch
   the live count, then **Reveal** → leaderboard → **Next**. Last two questions are **double
   points**. Winner + confetti at the end.

Players can join on a phone or a second browser tab — anything with a browser. No sign-in.

---

## Make it yours

Everything is in `index.html`:

- **Questions** — edit the `QUESTIONS` array. Each entry: `cat`, `prompt`, four `options`,
  and `correct` (the index 0–3 of the right option). Add `dbl:true` for a double-point
  question. Set `art:'horror'` or `art:'music'` to show the illustration.
- **Timer** — default seconds are chosen in the lobby; change the default in the code by
  editing `qSeconds:20`.
- **Look** — colors are CSS variables in `:root` (Sapience palette). The `sapience ai` mark
  is in the footer.

---

## Notes & limits

- The Firebase **free (Spark) tier** is plenty for a team event (generous simultaneous
  connections and daily data). No credit card required.
- Rooms are public to anyone who has the code — fine for a casual game; don't put anything
  sensitive in it.
- The host's screen is the source of truth. If the host refreshes mid-game, it resumes; if
  the host closes the tab for good, use **End room** first or just start a fresh room.

Made by AK · Sapience AI
