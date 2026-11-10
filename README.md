# bot-video-engines

Public release candidate of two video engines. Tag `rc1`. Not recipient-tested. This repository does not include a test log.

Owner-written code in each engine is MIT, copyright Adem Vessell. That grant does not cover third-party works. Remotion is not MIT. Font licenses stay with the font files. There is no repository-wide license that relicenses those works.

| Engine | Tree | Release archive |
|---|---|---|
| Talking-head director 0.1.0-rc1 | `talking-head-director-0.1.0-rc1/` | `talking-head-director-0.1.0-rc1.tar.gz` |
| Viz explainer starter v1 | `viz-explainer-starter-v1/` | `viz-explainer-starter-v1.tar.gz` |

Download URLs, after the `rc1` release upload:

- https://github.com/AdemVessell/bot-video-engines/releases/download/rc1/talking-head-director-0.1.0-rc1.tar.gz
- https://github.com/AdemVessell/bot-video-engines/releases/download/rc1/viz-explainer-starter-v1.tar.gz

Pins are in `SHA256SUMS` and `MANIFEST.json`. Treat the archives as published only when `bootstrap/published.json` is present and its hashes match a download. See `BOOTSTRAP.md`.

## Talking-head director

Two separate paths:

- **Source-aligned** (`scripts/aligned/render.py`): manual segment list and word map. The frame is the fixed workshop mat, one print of the current frame, and tape. Optional waveform, colour marker (`track`), and diagram. No packaged arbitrary matting and no generated scenes on this path. No speech recognition.
- **`scripts/thd`**: a different editor (cut, captions, house styles, synthesised sound, checks). It can build styled frames. It is not the source-aligned frame. It never posts.

The example under `examples/source-aligned/talk/` is synthetic speech and a flat picture. It is not the author's footage.

Setup: Python 3.9+, ffmpeg, and `requirements.txt`. Linux install uses `scripts/install-linux-cpu.sh`, which creates a virtualenv and downloads torch and torchaudio CPU wheels. The first prep, autocut, or selftest downloads Whisper `medium.en` (about 1.5 GB). No Mac, private account, or connector is required. Details: `talking-head-director-0.1.0-rc1/docs/INSTALL-linux-cpu.md`.

## Viz explainer starter

A Remotion + Three.js starter. `node setup.mjs <new-or-empty-dir>` checks Node >= 18, npm, ffmpeg, and ffprobe, copies into an empty directory, and runs `npm ci`. The first render downloads Remotion's headless browser. No Mac, private account, or connector is required.

Remotion's license is `viz-explainer-starter-v1/LICENSES/REMOTION-LICENSE.md`. Read that file before commercial use. three.js is MIT. Bundled fonts are SIL OFL 1.1.

## What this publication does not claim

No recipient has been recorded as testing these archives here. Dependency installs and model or browser downloads are required for a full run and are not bundled. Nothing in these trees posts, uploads a render, or calls a paid voice or music API.
