# converter-filex

A fork of [p2r3/convert](https://github.com/p2r3/convert) (GPL-2.0) with a small
patch so [filex](https://github.com/brf-tech/filex) can embed the converter in a
hidden iframe for in-browser file conversion.

## What's different from upstream

One file — `src/main.ts` — adds an embed bridge that is active only with
`?embed=1`:

- `setupEmbedBridge()` — a `postMessage` protocol (`listFormats`, `convert`) so
  a host page can drive conversions headlessly and get the bytes back.
- `buildOptionsFromCache()` — builds the format graph from the precached
  `cache.json` without per-handler init (avoids a load hang in a hidden iframe).
- a `requestAnimationFrame`→`setTimeout` shim so conversions run at full speed
  when the iframe is throttled.

Everything else is upstream. All conversion still happens in the browser (WASM);
no files are uploaded to any server.

## Image

Pushes to `master` publish `ghcr.io/brf-tech/converter-filex:latest`
(see `.github/workflows/docker.yml`). Point filex at it:

```
# served under /convert on your filex host
FILEX_CONVERT_URL=https://<your-host>/convert
# in the filex compose full stack:
CONVERT_IMAGE=ghcr.io/brf-tech/converter-filex:latest
```

## License

GPL-2.0, inherited from upstream — see [LICENSE](LICENSE).
