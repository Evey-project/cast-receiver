# cast-receiver

Web Receiver Google Cast custom d'Evey, servi sur https://cast.eveyproject.org/
(GitHub Pages, domaine custom - cette URL est celle déclarée dans la Google
Cast Console : elle ne doit JAMAIS changer).

Le receiver sonde les capacités réelles du Chromecast (canDisplayType),
re-négocie le flux HLS avec l'instance Evey de l'utilisateur, décode
l'AC-3/E-AC-3 sur la TV via ac3go (WebAssembly, licence PolyForm Noncommercial,
voir wasm/ac3go.LICENSE.md) et affiche les erreurs de lecture à l'écran.

## Regénérer depuis le dépôt evey

```bash
docker exec evey-web sh -c 'cd /app && pnpm --filter web run build:cast-receiver'
cp -r frontend/apps/web/dist-cast-receiver/assets <ce-repo>/assets
cp frontend/apps/web/dist-cast-receiver/cast-receiver.html <ce-repo>/index.html
cp -r frontend/apps/web/public/wasm <ce-repo>/wasm
```
