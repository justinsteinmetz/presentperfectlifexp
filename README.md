# LIFE XP — Present Perfect Speaking Game

**Live:** https://justinsteinmetz.github.io/presentperfectlifexp/

A fast-paced classroom game for practising the present perfect. Students claim experience cards by typing grammatically correct sentences against the clock. Supports solo play with a personal best tracker and live multiplayer via Ably.

---

## How it works

Each card shows an emoji, a verb prompt, and an experience phrase — for example:

> 🍕 **eat** · pizza for breakfast

The student must type the correct present perfect sentence:

> *I have eaten pizza for breakfast.*

Cards are organised into five categories: Food & Drink, Travel, Sports, Culture, and Wild Cards. Each card carries an XP value. Students score full XP on the first correct attempt and half XP on the second. Two failed attempts locks the card.

---

## Game modes

### Solo
Enter a name and play alone against a 3-minute timer. Score is compared to your personal best (stored locally per name). Results screen shows total XP, sentences claimed, personal best, and the difference from your best run.

### Multiplayer — Student
Enter the room code shown on the board and your name. Wait for the host to start. Play proceeds simultaneously with all students. A mini leaderboard strip at the bottom of the screen updates live.

### Multiplayer — Host
Creates a room and displays a code for students to join. The host sees a live full-screen leaderboard only — no cards, no gameplay. Controls: pause/resume the timer, end the game early. On game end, results are broadcast to all students simultaneously.

---

## Scoring

| Attempt | Result | XP awarded |
|---|---|---|
| 1st | Correct | Full XP |
| 2nd | Correct | Half XP |
| 2nd | Wrong | Card locked |

A floating grammar reference panel is visible during play showing the `have / has + past participle` rule.

---

## Answer validation

Input is normalised before comparison: trimmed, lowercased, punctuation stripped, whitespace collapsed. Students do not need to worry about capital letters or full stops — only the words matter.

---

## Architecture

Single HTML file. No build step, no framework, no backend.

- **Realtime multiplayer** — [Ably](https://ably.com/) pub/sub over WebSocket
- **Personal best storage** — `localStorage`, keyed per player name
- **Deployment** — GitHub Pages (static, drop-in)

The file is structured in clearly marked sections:

```
<style>          CSS (design tokens, screen layouts, card states)
<body>           HTML (four screens: lobby, waiting, game, results)
<script>
  CONSTANTS      Ably key, game duration
  CATEGORIES     Card data (emoji, verb, phrase, sentence, XP)
  STATE          Runtime variables
  HELPERS        Utility functions
  SOLO           Solo start/quit flow
  ABLY           Connection initialisation
  HOST           Room creation and control
  JOIN           Student join flow
  MESSAGE HANDLER  Ably event routing
  BEGIN GAME     Screen setup by mode
  TIMERS         Solo, host, and student timer loops
  LEADERBOARD    Build, render, update functions
  CATEGORIES UI  Sidebar and card rendering
  CARDS          Click, input, submit, validation logic
  RESULTS        Solo and multiplayer results screens
```

---

## Deployment

1. Fork or clone the repository
2. Enable GitHub Pages on the `main` branch from the root
3. The game is live at `https://[username].github.io/[repo-name]/`

No npm, no config, no build pipeline.

---

## Customising the cards

Card data lives in the `CATEGORIES` array near the top of the `<script>` block. Each card has five fields:

```js
{ emoji:'🍕', verb:'eat', phrase:'pizza for breakfast', sentence:'I have eaten pizza for breakfast.', xp:10 }
```

- `emoji` — displayed on the card face
- `verb` — shown as the prompt label (`have eat`)
- `phrase` — the experience description
- `sentence` — the exact correct answer (used for validation)
- `xp` — points awarded for a first-attempt correct answer

Add, remove, or edit cards freely. `MAX_XP` and `MAX_SENTENCES` are computed automatically.

---

## Multiplayer note

The Ably API key in the source is a free-tier key. For production use, replace it with your own key from [ably.com](https://ably.com/). Free tier supports up to 200 concurrent connections.

