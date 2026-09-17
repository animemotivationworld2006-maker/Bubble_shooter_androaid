This folder is intentionally empty in the source project.

phaser.min.js (Phaser 3.70.0) is downloaded into this folder automatically
by the GitHub Actions workflow (.github/workflows/android-build.yml) during
every build, from:
  https://cdnjs.cloudflare.com/ajax/libs/phaser/3.70.0/phaser.min.js

This is done at build time (not committed to the repo) because:
- The development sandbox that prepared this project has no outbound
  network access to fetch third-party files.
- Vendoring it at build time keeps the repo small and always pulls a known,
  pinned Phaser version (3.70.0) rather than relying on a live CDN request
  from inside the installed Android app (better offline reliability once
  packaged, since the file becomes part of the app bundle instead of being
  fetched over the network every time the app opens).

If you ever build locally with real internet access instead of CI, just run:
  curl -o www/vendor/phaser.min.js https://cdnjs.cloudflare.com/ajax/libs/phaser/3.70.0/phaser.min.js
before running `npx cap sync`.
