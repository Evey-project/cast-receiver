# cast-receiver

Evey's custom Google Cast Web Receiver, served at https://cast.eveyproject.org/
(GitHub Pages behind a custom domain). That URL is the one registered in the
Google Cast Console: it must NEVER change.

The receiver probes the real capabilities of the Chromecast it runs on
(`canDisplayType`), renegotiates the HLS stream with the user's own Evey
instance so the television only gets codecs it decodes natively (audio rides
the server's AAC rendition), and shows playback errors on screen. Every segment
request carries CMCD (CTA-5004), so a cast shows up in the Evey admin's
playback-quality table under its own `cast` device class.

The on-TV AC-3 / E-AC-3 decoder (ac3go in WebAssembly) was removed from the
published receiver on 2026-10-10: it had been switched off on both sides since
2026-08-28 (`CAST_WASM_ENABLED` in the receiver, `EVEY_CAST_WASM_AUDIO_ENABLED`
on the server). The code path still exists in evey behind that switch; the
`wasm/` directory is in this repository's history (last at `a47925e`) should it
come back.

## Layout

- `index.html` - the receiver page (built from `cast-receiver.html` in evey).
- `assets/` - the receiver bundle.
- `CNAME` - the GitHub Pages custom domain.

The sources live in the evey repository, under
`frontend/apps/web/src/cast-receiver/`. This repository only holds the built
output.

## Regenerating from the evey repository

```bash
docker exec evey-web sh -c 'cd /app && pnpm --filter web run build:cast-receiver'
cp -r frontend/apps/web/dist-cast-receiver/assets <this-repo>/assets
cp frontend/apps/web/dist-cast-receiver/cast-receiver.html <this-repo>/index.html
```

Replace `assets/` wholesale (the bundle name carries a content hash). Pushing
`main` publishes the new receiver on GitHub Pages; the Chromecast loads it on
the next cast session.
