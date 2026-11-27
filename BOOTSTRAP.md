# Bootstrap

Which release to use:

| Engine | Release | Archive | Immutable? |
|---|---|---|---|
| Talking-head director | `rc2` | `talking-head-director-0.1.0-rc2.tar.gz` | Yes. GitHub reports `immutable: true` for `rc2`. |
| Viz explainer starter | `rc1` | `viz-explainer-starter-v1.tar.gz` | **No.** `rc1` was published before release immutability was turned on, so its files and tag could still be changed by someone with write access. The pin below still catches a changed file. |

Do not reconstruct either engine from memory. `bootstrap/published.json` on `main` records the pins that the public download URLs returned. It is a later commit and is not inside any tar.gz. If `published.json` is missing, or its `sha256` values differ from the download, stop. The `rc1` and `rc2` git tags both point at source commit `f8a7703350307536d3c27f2e10437471d6d437c7`.

Do not use the talking-head `rc1` archive any more. It is superseded by `rc2`, which has the same engine content.

Release pages:

- https://github.com/AdemVessell/bot-video-engines/releases/tag/rc2 (talking-head director)
- https://github.com/AdemVessell/bot-video-engines/releases/tag/rc1 (viz explainer starter)

## Fetch and check

Expected pins (also in `SHA256SUMS` and `MANIFEST.json`):

```text
7673fefe2e4ba1499f15c9960eba4d28ce853a8848114e9b9b4c5f4b1609565b  talking-head-director-0.1.0-rc2.tar.gz   (1366594 bytes)
873d063ffe1f69657b219721ef705fa9a76f0ee78916ed0a36492d18915f0a3b  viz-explainer-starter-v1.tar.gz   (202879 bytes)
```

Quick manual check. Use `sha256sum` on Linux or `shasum -a 256` on macOS, and compare the hash by eye or with `[ "$a" = "$b" ]`:

```bash
base=https://github.com/AdemVessell/bot-video-engines/releases/download
curl -fsSL -o talking-head-director-0.1.0-rc2.tar.gz "$base/rc2/talking-head-director-0.1.0-rc2.tar.gz"
curl -fsSL -o viz-explainer-starter-v1.tar.gz "$base/rc1/viz-explainer-starter-v1.tar.gz"
for f in talking-head-director-0.1.0-rc2.tar.gz viz-explainer-starter-v1.tar.gz; do
  if command -v sha256sum >/dev/null 2>&1; then sha256sum "$f"; else shasum -a 256 "$f"; fi
done
```

Bots should not rely on this snippet. A bot should use a verify script that also checks the byte size, rejects unsafe archive paths and links, and refuses on any mismatch without trying another URL. Keep that script in the bot's own runtime skill.

`SHA256SUMS` in this repository lists the two current archives. The `SHA256SUMS` file attached to the `rc2` release lists the talking-head archive only. The one attached to `rc1` lists both `rc1` archives. None of these files hashes itself or `MANIFEST.json`, and they sit in the same release as the archives, so they are not an independent check. The pin is what you compare against.

## What a pin covers

A pin covers the bytes of the archive you download. It does not cover the text of the skill that holds the pin, and it does not cover anything the engine downloads or installs after unpacking (Python packages, torch wheels, Whisper weights, npm packages, ffmpeg).

## Talking-head director

```bash
tar -xzf talking-head-director-0.1.0-rc2.tar.gz
cd talking-head-director-0.1.0-rc2
# optional, and it does download packages:
# bash scripts/install-linux-cpu.sh
```

The files inside the `rc2` archive still say `0.1.0-rc1` in their text. The content is the same as `rc1`; only the top folder name changed. `BUILD.md` records exactly how `rc2` was built.

Requirements before a real run: Python 3.9+, ffmpeg and ffprobe, and the pins in `requirements.txt`. The Linux installer creates a virtualenv and downloads torch and torchaudio CPU wheels from `https://download.pytorch.org/whl/cpu`. The first `prep`, `autocut`, or `selftest` downloads Whisper `medium.en` (about 1.5 GB). No Mac, private account, or connector is required to fetch and verify the archive. The install and run steps are Linux only.

Source-aligned use is manual: a segment list plus a word map, then `python3 scripts/aligned/render.py PLAN preview`. The frame is the fixed mat, one print, and tape. Waveform, marker, and diagram are optional plan keys. `scripts/thd` is the other path. Read `docs/SOURCE_ALIGNED.md` and `docs/INSTALL-linux-cpu.md`. The talk example is synthetic and is not the author's footage.

## Viz explainer starter (still on rc1, not immutable)

```bash
tar -xzf viz-explainer-starter-v1.tar.gz
cd viz-explainer-starter-v1
node setup.mjs ../my-explainer
```

`setup.mjs` refuses a non-empty destination. It runs `npm ci` unless you pass `--no-install`. The first Remotion render downloads a headless browser. Needs Node >= 18, npm, ffmpeg, and ffprobe. Remotion is not MIT. Read `LICENSES/REMOTION-LICENSE.md`.

## Updating a pin

Every new version gets a new tag, for example `rc3`. Never edit or re-upload files on an existing tag. Before a bot's pin changes, re-download the new archive from its published URL, re-hash it, compare it with `bootstrap/published.json` and the release's reported asset digest, and confirm the release shows `immutable: true`. Then change the bot's skill and `published.json` together. Bots created from an older template keep the old pin until their owners update them.

Nothing here has been tested by a recipient on a fresh machine.
