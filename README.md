# music.txt

*[日本語](README.ja.md)*

A small toolkit for embedding music into a blog post: paste your text, mark where a track should play with `[[URL]]`, and get back plain HTML you can drop into any blog editor. No coding required.

**Live pages**
- [Generator (English)](https://otaotaotao.github.io/music.txt/generator-en.html)
- [ジェネレーター（日本語）](https://otaotaotao.github.io/music.txt/generator.html)
- [Sample post](https://otaotaotao.github.io/music.txt/sample-walk.html) — a short walking-diary post with three tracks embedded, crossfading as you scroll

## How it works

1. Paste your post text into the generator.
2. Wherever you want a track, write `[[https://... some URL ...]]` — inline in a sentence or on its own line, either is fine.
3. Pick a switching mode and click Generate.
4. Copy the HTML it produces into your blog's HTML/source editor.

The first `[[URL]]` in the text becomes a play/pause button. Every one after that becomes a track-switch point, numbered to match a track list the generator can append to the end of the post automatically (fetched from each service, so no manual typing of titles).

## Supported services

| Service | Playback | Visible on the page |
|---|---|---|
| YouTube | Full control: hidden player, fades, auto or manual crossfade | Nothing (invisible) |
| SoundCloud | Same as YouTube | Nothing (invisible) |
| Spotify | Native controls only | A normal embedded player, shown inline |
| Apple Music | Native controls only | A normal embedded player, shown inline |
| Bandcamp | Native controls only | A normal embedded player, or a plain link if no embed could be generated |

YouTube and SoundCloud both expose a JS API with volume control, which is what makes the hidden-player, fade, and auto-crossfade behavior possible. Spotify and Apple Music intentionally don't expose volume control through their embed APIs, and Apple Music's programmatic control (MusicKit JS) requires a paid developer account — so links to those three services are rendered as normal, visible players instead, using each service's own play button.

## Switching modes

- **Manual only** — a track only changes when the reader taps the switch button.
- **Auto transition** (default) — scrolling to a track's paragraph crossfades automatically; a manual button is still shown as a fallback.
- **Tap to reveal** — the post starts collapsed to its first paragraph; tapping "▼" reveals the next one, and track switches happen in step with that tap instead of with scrolling. This changes the reading experience itself, not just the audio.

## A note on iOS

Mobile Safari has a real quirk here: it silently ignores JS volume changes on embedded video/audio (this is a platform restriction, not a bug in this project), and the very first `play()` call on a YouTube/SoundCloud iframe sometimes needs to be retried before it actually starts. The generated script works around both — muting/unmuting instead of a smooth fade on iOS, and retrying playback a few times before giving up — but this remains one of the rougher edges of embedding third-party players. If a transition ever stays silent, the always-visible master button doubles as a manual "make sure it's actually playing" control.

## Files

- `generator.html` / `generator-en.html` — the tool itself (Japanese / English UI, identical logic)
- `sample-walk.html` — an example post produced with the generator
- `index.html` — links to the above
