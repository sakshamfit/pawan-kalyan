# OG — They Call Him OG

A Pawan Kalyan showcase website. Complete cinematic OG fan website: logo intro, scroll-driven circular transitions, rotating image archive, red crescent, galleries, film chapters, animated footer, responsive layouts and 55-second showcase mode.

## Where the showcase is documented (in code comments)

The whole showcase is annotated in the source, so the site can be read end to end without running it:

| File | Comment block covers |
| --- | --- |
| `app/page.tsx` | Master header: page structure, the five chapters, the scrubbed GSAP timeline marks, media layering, motion-safety and accessibility. Plus per-chapter and per-animation inline notes. |
| `app/layout.tsx` | Root shell, palette and document metadata. |
| `app/globals.css` | Stylesheet map: palette tokens, showcase mode, base/a11y, every scene class, responsive blocks. |
| `components/cinema-media.tsx` | The three-layer moving-image aperture (poster still → bundled MP4 → muted YouTube) and the handover/playback rules. |
| `components/showcase-controller.tsx` | The 55-second guided tour: cue sheet, monotone cubic interpolation, start/stop conditions, tab recording. |
| `lib/cinema-playback.ts` | YouTube transport state machine behind the loops. |
| `lib/youtube-player.ts` | Shared single loader for the YouTube iframe API. |
| `tools/video/playback-regression.test.mjs` | Invariants the playback tests assert. |
| `tools/video/render-showcase.mjs` | Offline renderer that mirrors the journey from committed stills. |
| `next.config.ts` | Why `allowedDevOrigins` carries the preview hosts. |

## Deploy to Vercel


Import `gireeshkumarreddy/pk` and use the repository root. The included `vercel.json` selects Next.js, installs the pinned pnpm dependencies and runs the production build. No environment variables or API keys are required for this presentation site.

- Framework: Next.js
- Node: 22.13 or newer (`.nvmrc` selects 22)
- Install: `pnpm install --frozen-lockfile`
- Build: `pnpm build`
- Output directory: framework default (leave unset)

The previous ChatGPT-hosted URL has its own access settings. An external deployment uses the access settings of the chosen hosting provider.

## Run locally or on another Node host

```sh
corepack enable
pnpm install --frozen-lockfile
pnpm dev
```

For production, run `pnpm build` then `pnpm start`. Set the host's `PORT` if needed. This is a Next.js project, so uploading only source files to a static file host is not sufficient.

To serve the showcase from a sandbox or container preview, bind to all interfaces — `scripts/run-next-dev.mjs` maps `--host` onto Next's `--hostname`:

```sh
pnpm dev --host 0.0.0.0 --port 3000
```

`next.config.ts` already allows the `*.e2b.app` preview origin, so proxied previews load dev assets instead of hitting a 403.


## Included files

- `app/`: full website, animation timeline, styles and metadata.
- `components/`, `lib/`, `hooks/`: player, showcase controls and UI source.
- `public/media/`: all 17 website images/title artworks and 5 native MP4 clips in `video/`.
- `docs/`: animation reference, asset provenance, local-video sources and quality notes.
- `tools/video/`: original artwork renderer and playback regression checks.
- `pnpm-lock.yaml`: dependency lockfile.
- Original Vinext/Sites files and scripts are preserved. `dev:sites`, `build:sites`, and `start:sites` retain that workflow; default scripts use Next.js for external hosting.

## Videos and autoplay

The original official YouTube excerpts and full-film modal links are preserved. Bundled, muted, inline MP4 loops start when their panels become visible and remain visible while YouTube loads. Once a YouTube excerpt has demonstrably advancing playback, its layer replaces the local loop. Off-screen, hidden-tab and globally paused media stop; opening a film modal also pauses background clips.

The entrance uses `katana.mp4`; the wide action panel and Hungry Cheetah chapter use `entrance.mp4`; the Firestorm chapter and footer use `firestorm.mp4`; the trailer chapter uses the katana teaser while its official stream loads. `title.mp4` and `glasses.mp4` are additional retained source clips.

**Media limitation:** these bundled MP4s are short standard-definition promotional loops (498 pixels wide), not complete trailers or native HD/4K masters. Full-length films and higher-resolution playback still require YouTube/network access. The local clips are real files committed to Git, with no Git LFS or runtime download step. Poster fallbacks remain if a browser blocks autoplay or prefers reduced motion.

Open `?showcase=1` for the existing 55-second guided tour and optional tab recorder. The tour waits for actual advancing footage, including the bundled clips.

## Verify

```sh
pnpm typecheck
pnpm test:playback
pnpm build
```

`docs/QA.md` contains historical hosted-version notes; `docs/GITHUB-HANDOFF.md` records this portable edition. Development caches, dependencies, credentials and generated build outputs are excluded; they are recreated at install/build time.

This is an independent fan experience. Movie imagery, title marks and music belong to their respective owners. Promotional availability does not establish a general reuse license; source links are recorded in `docs/`.
