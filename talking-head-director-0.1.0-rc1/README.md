# Talking-head director 0.1.0-rc1

Release candidate. Not recipient-tested. This publication does not include a test log.

Two paths ship in this tree. They are different commands.

## Source-aligned path

`scripts/aligned/render.py` cuts one video you already have. You supply the segment list and a word map (`{w, s, e}`). The frame is the fixed workshop mat, one print of the current frame, and tape. Optional plan keys draw a waveform, a colour marker (`track`), or a diagram. This path does not ship arbitrary matting or generated scenes. It does not run speech recognition. See `docs/SOURCE_ALIGNED.md`.

`examples/source-aligned/talk/` is synthetic speech on a flat picture. It is not the author's footage.

## Other path: `scripts/thd`

`scripts/thd auto` is a separate editor for a talking-head recording: fillers and dead air cut, shot changes, word-timed captions, papers on the speaker's words, designed sound, then a loudness, sync, and pre-posting check. `thd styles` names the house looks. That path can build those styled frames. It is not the source-aligned mat / one-print / tape frame. The tool never posts.

```text
scripts/thd doctor                      check this machine
scripts/thd styles                      the looks
scripts/thd auto RECORDING              hands-free edit -> RECORDING_edit/deliver/
scripts/thd auto RECORDING --draft      list the lines, to write a brief from
scripts/thd auto RECORDING --brief brief.yaml
```

`thd auto --help` and `scripts/thd_engine/auto.py` describe the brief. `docs/WORKFLOW.md` is the edit order for this path. `bash scripts/thd` works when the executable bit is missing.

## Setup

Needs Python 3.9+, ffmpeg (ffprobe, libx264, prores_ks, aac, drawtext), and the packages in `requirements.txt`. No Mac, private account, or connector is required.

Linux install is an isolated virtualenv. It downloads pinned CPU wheels for torch and torchaudio from `https://download.pytorch.org/whl/cpu`, then the other pins. The first `prep`, `autocut`, or `selftest` downloads Whisper `medium.en` weights (about 1.5 GB) into the Whisper cache. That is a model download, not a paid API. See `docs/INSTALL-linux-cpu.md` and `scripts/install-linux-cpu.sh`.

macOS `say` and Linux `espeak-ng` are only for the synthetic `thd selftest` voice. System fonts are an optional fallback. The faces this package draws are the open fonts in `assets/fonts`. Apple's sound library is not shipped. Missing cues are synthesised and have not been approved by ear.

## Rights

Owner-written code and owner-written docs are MIT, copyright Adem Vessell (`LICENSE`). That grant does not relicense the fonts, Python packages, Whisper weights, ffmpeg, or espeak-ng. Third-party terms stay in `NOTICES.md` and `assets/fonts/licenses/`.

Public archive, once the `rc1` release is uploaded:

`https://github.com/AdemVessell/bot-video-engines/releases/download/rc1/talking-head-director-0.1.0-rc1.tar.gz`

Confirm the hash in the repository `SHA256SUMS` before use. `bootstrap/published.json` is the signal that those bytes were checked on the public URL.
