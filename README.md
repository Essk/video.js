# video.js PR #9237 caption layout check

Live page: https://essk.github.io/video.js/

A reduced page for https://github.com/videojs/video.js/pull/9237. It loads the built
`dist/video.js` straight from the Netlify deploy previews (PR build or `main` build),
forces `CSS.supports('inset', ...)` to return `false` before video.js loads so the
no-`inset` fallback path in `TextTrackDisplay#updateDisplay` runs on any modern browser,
and renders a player with a captions track.

Query parameters:

- `build=pr` (default) or `build=main`
- `mode=fixed` (default, `width`/`height` attributes) or `mode=fluid` (`vjs-fluid` class)

To exercise the `IS_SMART_TV` branch in the `main` build on desktop Chrome, spoof a
Tizen user agent and remove client hints before load, for example via DevTools
device emulation or an init script:

```js
Object.defineProperty(navigator, 'userAgent', { get: () => 'Mozilla/5.0 (SMART-TV; LINUX; Tizen 6.0) AppleWebKit/537.36 (KHTML, like Gecko) 76.0.3809.146/6.0 TV Safari/537.36' });
Object.defineProperty(navigator, 'userAgentData', { get: () => undefined });
```

The PR build ignores that flag, so no spoofing is needed for `build=pr`.

| Build | Sizing | Caption layer position |
|---|---|---|
| main (Tizen UA) | fixed | one player height above the player |
| main (Tizen UA) | fluid | in frame |
| PR #9237 | fixed | in frame |
| PR #9237 | fluid | one player height below the player |

The dashed yellow box marks the player's bounds.
