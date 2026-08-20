# КОЛОБОК — вдали от сборов / site kit

Assets and a single build prompt for the promo page of a fictional film.

**Live reference:** https://kolobok-vdali-ot-sborov.vercel.app

## How to use

1. Clone this repo (or download the ZIP):

   ```bash
   git clone https://github.com/keizgraphics/kolobok-site-kit.git
   cd kolobok-site-kit
   ```

2. Open it with a coding agent that has a shell — Claude Code, Codex CLI, Cursor.
3. Paste the contents of `PROMPT.md` as your task.
4. The agent writes `index.html`, `styles.css`, `hero.js` around the assets that are
   already here, then serves the page locally.

A chat window without a terminal will not finish this: the page needs local files
and a Range-capable static server for the video.

## What's inside

```
assets/          26 files — sky, island, letters, characters, icons, video, poster
fonts/           the display typeface
manifest.txt     flat list of every asset path
PROMPT.md        the full build spec: tokens, geometry, motion, responsive, acceptance
```

Nothing here runs code. There is no build step, no npm, no dependencies —
the finished site is three plain files plus these assets.
