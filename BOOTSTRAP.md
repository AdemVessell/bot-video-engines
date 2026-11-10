# Bootstrap

Use the `rc1` release archives. Do not reconstruct either engine from memory. Do not treat the links as published until `bootstrap/published.json` exists in this repository and the file's `sha256` values match the bytes you downloaded.

Release page: https://github.com/AdemVessell/bot-video-engines/releases/tag/rc1

## Fetch and check

```bash
base=https://github.com/AdemVessell/bot-video-engines/releases/download/rc1
curl -fsSL -o talking-head-director-0.1.0-rc1.tar.gz "$base/talking-head-director-0.1.0-rc1.tar.gz"
curl -fsSL -o viz-explainer-starter-v1.tar.gz "$base/viz-explainer-starter-v1.tar.gz"
curl -fsSL -o SHA256SUMS "$base/SHA256SUMS"
sha256sum -c SHA256SUMS
```

Expected pins (also in `SHA256SUMS` and `MANIFEST.json`):

```text
99bd1108476b1a1da5b8e6ae67b8e88fe90f7655594ceac213750bd674c7ee06  talking-head-director-0.1.0-rc1.tar.gz
873d063ffe1f69657b219721ef705fa9a76f0ee78916ed0a36492d18915f0a3b  viz-explainer-starter-v1.tar.gz
```

`SHA256SUMS` lists those two archives only. It does not hash itself or `MANIFEST.json`.

## Talking-head director

```bash
tar -xzf talking-head-director-0.1.0-rc1.tar.gz
cd talking-head-director-0.1.0-rc1
# optional, and it does download packages:
# bash scripts/install-linux-cpu.sh
```

Requirements before a real run: Python 3.9+, ffmpeg and ffprobe, and the pins in `requirements.txt`. The Linux installer creates a virtualenv and downloads torch and torchaudio CPU wheels from `https://download.pytorch.org/whl/cpu`. The first `prep`, `autocut`, or `selftest` downloads Whisper `medium.en` (about 1.5 GB). No Mac, private account, or connector is required.

Source-aligned use is manual: a segment list plus a word map, then `python3 scripts/aligned/render.py PLAN preview`. The frame is the fixed mat, one print, and tape. Waveform, marker, and diagram are optional plan keys. `scripts/thd` is the other path. Read `docs/SOURCE_ALIGNED.md` and `docs/INSTALL-linux-cpu.md`. The talk example is synthetic and is not the author's footage.

## Viz explainer starter

```bash
tar -xzf viz-explainer-starter-v1.tar.gz
cd viz-explainer-starter-v1
node setup.mjs ../my-explainer
```

`setup.mjs` refuses a non-empty destination. It runs `npm ci` unless you pass `--no-install`. The first Remotion render downloads a headless browser. Needs Node >= 18, npm, ffmpeg, and ffprobe. Remotion is not MIT. Read `LICENSES/REMOTION-LICENSE.md`.

## Same-link update

Replace a previous archive URL with the two download URLs above and the two SHA256 pins in this file. Keep using tag `rc1` and these filenames. If `bootstrap/published.json` is missing, or its hashes differ from the download, stop and do not switch the bot to these archives.
