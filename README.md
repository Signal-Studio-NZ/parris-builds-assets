# Parris Builds - website assets

Public media assets (video, etc.) for the Parris Builds website. Kept public on purpose so they can be served over a CDN.

## How these are served

Files here are delivered via the free **jsDelivr** CDN (globally cached, supports range requests for smooth video seeking). For any file in this repo, the URL pattern is:

```
https://cdn.jsdelivr.net/gh/Signal-Studio-NZ/parris-builds-assets@main/<path>
```

Example, for a file named `testimonial-1.mp4` in the repo root:

```
https://cdn.jsdelivr.net/gh/Signal-Studio-NZ/parris-builds-assets@main/testimonial-1.mp4
```

That URL is what gets pasted into the FlowPlay video player in Webflow (self-hosted / custom video source). For a URL going onto a live page, prefer pinning to a commit SHA or tag over `@main` (see caching notes below).

## Conventions

- **Keep filenames and commit messages neutral.** No personal names or identifying context in filenames, commits, or this README.
- **Videos must be web-optimized MP4** (H.264 + AAC, `-movflags +faststart`) and **under 20MB**. jsDelivr enforces a **20MB per-file limit for GitHub repos** (a file over that returns `403 File size exceeded the configured limit of 20 MB`). 20MB also satisfies FlowPlay's 25MB GitHub guideline, so 20MB is the number to design around.
  - Reliable ffmpeg recipe for a ~47s 1080p clip landing ~18MB (two-pass): `-c:v libx264 -b:v 3000k -preset slow -c:a aac -b:a 128k -movflags +faststart`. Scale the video bitrate to length: roughly `bitrate_kbps = (target_MB * 8192 / duration_seconds) - 128`.
- **Caching / updating a file.** jsDelivr caches both the file and the `@main` -> latest-commit resolution (up to ~12h), so a re-pushed or overwritten file may keep serving the old version for a while. Options:
  - **Immediate + immutable:** reference a commit SHA, e.g. `@<sha>/<path>` (always serves that exact version, never cached-stale). Best for a URL you paste into a live site.
  - **Clean + versioned:** create a git tag (e.g. `v1`) and use `@v1/<path>`; bump the tag to publish a new version.
  - **Purge `@main`:** `https://purge.jsdelivr.net/gh/Signal-Studio-NZ/parris-builds-assets@main/<path>` (can still lag behind the branch-pointer cache).
  - A **brand-new filename** avoids all of this, as long as `@main` has already refreshed to a commit that contains it.

## Notes

Anything committed here is publicly viewable and downloadable. Only put assets here that are meant to be public (they are embedded on the public website anyway). For anything that needs real access control, use a host with signed URLs (e.g. Cloudflare Stream) instead.
