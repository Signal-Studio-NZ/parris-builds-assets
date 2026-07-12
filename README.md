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

That URL is what gets pasted into the FlowPlay video player in Webflow (self-hosted / custom video source).

## Conventions

- **Keep filenames and commit messages neutral.** No personal names or identifying context in filenames, commits, or this README.
- **Videos must be web-optimized MP4** (H.264 + AAC, `-movflags +faststart`) and **under 25MB** (FlowPlay's GitHub limit; also well under jsDelivr's 50MB gh limit).
- jsDelivr caches `@main` aggressively. To force an update after replacing a file, either use a git tag (e.g. `@v1`) in the URL and bump it, or purge the file at `https://purge.jsdelivr.net/gh/Signal-Studio-NZ/parris-builds-assets@main/<path>`.

## Notes

Anything committed here is publicly viewable and downloadable. Only put assets here that are meant to be public (they are embedded on the public website anyway). For anything that needs real access control, use a host with signed URLs (e.g. Cloudflare Stream) instead.
