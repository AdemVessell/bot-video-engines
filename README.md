# bot-video-engines

Public release candidates of two video engines. The talking-head director is in release `rc2`, which GitHub reports as immutable. The viz explainer starter is still in release `rc1`, which is **not** immutable (it was published before that setting was turned on; the SHA-256 pin still detects a changed file). Neither is recipient-tested. This repository does not include a test log.

Owner-written code in each engine is MIT, copyright Adem Vessell. That grant does not cover third-party works. Remotion is not MIT. Font licenses stay with the font files. There is no repository-wide license that relicenses those works.

| Engine | Tree | Release archive |
|---|---|---|
| Talking-head director 0.1.0 | `talking-head-director-0.1.0-rc1/` (source folder; the `rc2` archive repackages it) | `talking-head-director-0.1.0-rc2.tar.gz`, release `rc2`, immutable |
| Viz explainer starter v1 | `viz-explainer-starter-v1/` | `viz-explainer-starter-v1.tar.gz`, release `rc1`, not immutable |

The superseded talking-head archive `talking-head-director-0.1.0-rc1.tar.gz` (release `rc1`) has the same engine content. Do not pin it.

Download URLs:

- https://github.com/AdemVessell/bot-video-engines/releases/download/rc2/talking-head-director-0.1.0-rc2.tar.gz
- https://github.com/AdemVessell/bot-video-engines/releases/download/rc1/viz-explainer-starter-v1.tar.gz

Pins are in `SHA256SUMS` and `MANIFEST.json`:

```text
7673fefe2e4ba1499f15c9960eba4d28ce853a8848114e9b9b4c5f4b1609565b  talking-head-director-0.1.0-rc2.tar.gz  (1366594 bytes)
873d063ffe1f69657b219721ef705fa9a76f0ee78916ed0a36492d18915f0a3b  viz-explainer-starter-v1.tar.gz  (202879 bytes)
```

Treat the archives as published only when `bootstrap/published.json` is present and its hashes match a download. A pin covers the archive bytes only. It does not cover the text of whatever skill holds the pin, or packages the engine installs afterwards. See `BOOTSTRAP.md`. `BUILD.md` records how the `rc2` archive was built.

## Talking-head director

Two separate paths:

- **Source-aligned** (`scripts/aligned/render.py`): manual segment list and word map. The frame is the fixed workshop mat, one print of the current frame, and tape. Optional waveform, colour marker (`track`), and diagram. No packaged arbitrary matting and no generated scenes on this path. No speech recognition.
- **`scripts/thd`**: a different editor (cut, captions, house styles, synthesised sound, checks). It can build styled frames. It is not the source-aligned frame. It never posts.

The example under `examples/source-aligned/talk/` is synthetic speech and a flat picture. It is not the author's footage.

Setup: Python 3.9+, ffmpeg, and `requirements.txt`. Linux install uses `scripts/install-linux-cpu.sh`, which creates a virtualenv and downloads torch and torchaudio CPU wheels. The first prep, autocut, or selftest downloads Whisper `medium.en` (about 1.5 GB). No Mac, private account, or connector is required. Details: `talking-head-director-0.1.0-rc1/docs/INSTALL-linux-cpu.md` (the source folder keeps its `rc1` name; inside the `rc2` archive it is `talking-head-director-0.1.0-rc2/docs/INSTALL-linux-cpu.md`).

## Viz explainer starter

A Remotion + Three.js starter. `node setup.mjs <new-or-empty-dir>` checks Node >= 18, npm, ffmpeg, and ffprobe, copies into an empty directory, and runs `npm ci`. The first render downloads Remotion's headless browser. No Mac, private account, or connector is required.

Remotion's license is `viz-explainer-starter-v1/LICENSES/REMOTION-LICENSE.md`. Read that file before commercial use. three.js is MIT. Bundled fonts are SIL OFL 1.1.

## What this publication does not claim

No recipient has been recorded as testing these archives here, and none has run them on a fresh machine. Dependency installs and model or browser downloads are required for a full run and are not bundled. Nothing in these trees posts, uploads a render, or calls a paid voice or music API.
