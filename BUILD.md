# Building the talking-head `rc2` archive

This records how `talking-head-director-0.1.0-rc2.tar.gz` (release `rc2`) was built, so anyone can rebuild it and compare hashes.

## What rc2 is

The same files as the superseded `rc1` talking-head archive, taken from source commit `f8a7703350307536d3c27f2e10437471d6d437c7` (folder `talking-head-director-0.1.0-rc1/`), packed under a new top folder `talking-head-director-0.1.0-rc2/`. File contents are unchanged, so text inside the archive still says `0.1.0-rc1`. It was repackaged only so the engine could be published as a GitHub immutable release, which `rc1` is not.

## Exact command

Build from the pinned commit, not from `main`. Docs on `main` changed after that commit, so a build from `main` gives different bytes.

```bash
git clone https://github.com/AdemVessell/bot-video-engines.git
cd bot-video-engines
git archive --format=tar \
  --mtime=2026-11-27T03:55:43Z \
  --prefix=talking-head-director-0.1.0-rc2/ \
  f8a7703350307536d3c27f2e10437471d6d437c7:talking-head-director-0.1.0-rc1 \
  | gzip -n -9 > talking-head-director-0.1.0-rc2.tar.gz
sha256sum talking-head-director-0.1.0-rc2.tar.gz   # shasum -a 256 on macOS
```

Expected:

```text
7673fefe2e4ba1499f15c9960eba4d28ce853a8848114e9b9b4c5f4b1609565b  talking-head-director-0.1.0-rc2.tar.gz  (1366594 bytes)
```

Uncompressed tar, which does not depend on the gzip version:

```bash
gzip -dc talking-head-director-0.1.0-rc2.tar.gz | sha256sum
# 0fedeb519bfc8bb5d7d5ef6686231ae8e7a6b47a62640d1822c2d4f7b03819a5
```

## Does a rebuild give the same hash?

Yes, if you use the command above, with these caveats.

- `--mtime` is required. Without it, `git archive` stamps every file with the time of the build, so two builds a few seconds apart give different hashes. The `--mtime` value above is the build time of the published `rc2` (2026-11-27T03:55:43Z, read back from the archive's own file times). It is not the commit time. A plain rebuild with no `--mtime` does not reproduce `rc2`.
- Measured on git 2.47.3 and GNU gzip 1.13: a fresh clone rebuilt the exact published bytes, and the result did not change with the time of day, the `TZ` setting or the `umask`.
- Other gzip versions or implementations can compress the same tar to different bytes. If the compressed hash differs, compare the uncompressed tar hash above. If that matches, the contents are the same and only the compression differs.
- `--mtime` needs git 2.39 or newer.

For a future release, use the commit time as `--mtime` so the date is not arbitrary, and record the command and the resulting hashes the same way. Never change files on an existing tag. Cut a new tag.

## Not covered

This shows that the archive bytes come from the commit above. It does not show that the engine works on a fresh machine. No recipient has tested it.
