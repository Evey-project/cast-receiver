# cast-receiver

Evey's custom Google Cast Web Receiver, served at https://cast.eveyproject.org/
(GitHub Pages behind a custom domain). That URL is the one registered in the
Google Cast Console: it must NEVER change.

The receiver probes the real capabilities of the Chromecast it runs on
(`canDisplayType`), renegotiates the HLS stream with the user's own Evey
instance, decodes AC-3 / E-AC-3 on the television through ac3go (WebAssembly,
AGPLv3, see `wasm/ac3go.LICENSE.md`), and shows playback errors on screen.
Every segment request carries CMCD (CTA-5004), so a cast shows up in the Evey
admin's playback-quality table under its own `cast` device class.

## Layout

- `index.html` - the receiver page (built from `cast-receiver.html` in evey).
- `assets/` - the receiver bundle.
- `wasm/` - the ac3go decoder, its worker and the Go runtime shim.
- `CNAME` - the GitHub Pages custom domain.

The sources live in the evey repository, under
`frontend/apps/web/src/cast-receiver/`. This repository only holds the built
output.

## Regenerating from the evey repository

```bash
docker exec evey-web sh -c 'cd /app && pnpm --filter web run build:cast-receiver'
cp -r frontend/apps/web/dist-cast-receiver/assets <this-repo>/assets
cp frontend/apps/web/dist-cast-receiver/cast-receiver.html <this-repo>/index.html
cp -r frontend/apps/web/public/wasm <this-repo>/wasm
```

Replace `assets/` wholesale (the bundle name carries a content hash). Only copy
`wasm/` when the decoder itself changed. Pushing `main` publishes the new
receiver on GitHub Pages; the Chromecast loads it on the next cast session.
