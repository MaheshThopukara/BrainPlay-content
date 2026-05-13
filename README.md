# BrainPlay-content

Public content packs for the BrainPlay Android TV app
([MaheshThopukara/BrainPlayKids](https://github.com/MaheshThopukara/BrainPlayKids)).

The app fetches `manifest.json` once per launch via jsDelivr's CDN, compares
each pack's `version` against the locally-seeded version, and downloads any
that are newer. SHA-256 hashes are verified before the new pack replaces
the previous one in the device's Room database.

## Why a separate repo

The main BrainPlayKids repo is private (proprietary game source). This
content repo is public so that jsDelivr can serve it — jsDelivr only
mirrors public GitHub repos.

## Layout

```
manifest.json                 ← entry point fetched on every launch
packs/
  emotion_detective.v2.json   ← 5,000 questions per bank
  what_would_you_say.v2.json
  odd_one_out.v2.json
  echo_tiles.v2.json
  mirror_match.v2.json
  clock_reader.v2.json
  pattern_detective.v1.json
  sentence_factory.v1.json
```

## CDN URLs

The manifest lives at:
```
https://cdn.jsdelivr.net/gh/MaheshThopukara/BrainPlay-content@main/manifest.json
```

Individual packs at:
```
https://cdn.jsdelivr.net/gh/MaheshThopukara/BrainPlay-content@main/packs/<bank>.v<N>.json
```

jsDelivr caches with a TTL of ~12 hours and serves from a global CDN.

## Publishing a new pack

1. Generate the pack in the main repo:
   ```bash
   python3 tools/content/expand_pack.py <bank_id> --version <N> --target 5000
   python3 tools/content/validate_pack.py --all
   ```
2. Copy the new JSON into this repo's `packs/` folder.
3. Update `manifest.json` — bump the `version` and replace `sizeBytes` +
   `sha256` for that bank (recompute with
   `shasum -a 256 packs/<bank>.v<N>.json`).
4. Commit + push. jsDelivr picks up the change automatically (purge with
   `https://purge.jsdelivr.net/gh/MaheshThopukara/BrainPlay-content@main/manifest.json`
   if you need the new pack within minutes instead of waiting for the
   TTL to expire).

## Banks NOT in this repo (yet)

The five remaining banks still run on the legacy in-app `*Bank.kt`
content generators bundled in the APK:

- Hidden Objects
- Daily Planner
- What Happens Next
- Read & Answer
- Maze Runner (procedural — likely never needs a content pack)

These will land here once their generators have been converted to
5,000-question packs. No app release is needed when they do — the sync
worker will pick up new entries in `manifest.json` and download them.
