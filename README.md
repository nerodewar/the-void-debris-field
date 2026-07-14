# THE VOID: Debris Field Module

A self-contained, GitHub Pages-ready iPad/desktop minigame prototype for **The Void**.

## Run it

Upload the contents of this folder to the root of a GitHub repository and enable **Settings → Pages → Deploy from branch**. The module uses only relative paths.

For local testing, serve the folder over HTTP. For example:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Controls

- iPad/touch: hold the virtual directional controls and hold **FIRE**.
- Desktop: WASD or arrow keys to steer; hold Space to fire.
- Text selection, image dragging, browser callouts and touch scrolling are disabled inside the game shell so held controls remain responsive.

## Rules implemented

- Two-minute run toward a 50-light-year endpoint.
- There is no game-over state. The sequence always produces a result.
- First meteorite impact drains the defence field.
- Another hit before defence recovery disables one engine for several seconds and slows distance gain.
- Small meteorite: 1 laser hit, 10 points.
- Medium meteorite: 3 laser hits, 35 points.
- Large meteorite: 6 laser hits, 80 points.
- Overall score = obliteration score × distance travelled.
- Results are graded as one, two or three stars.
- Laser heat prevents endless fire and rewards controlled bursts.
- Fast star streaks and progressively tighter meteor spawning create speed without unavoidable walls.

## Assets

- `assets/IMG00.png`: Luna/player portrait.
- `assets/IMGA1.png`: small pale meteorite.
- `assets/IMGA2.png`: medium orange meteorite.
- `assets/IMGA3.png`: large cosmic-blue meteorite.
- `assets/IMGA4.png`: player spaceship.

The four isolated gameplay assets have genuine transparent PNG backgrounds.

## Integration hooks

When the sequence finishes, the module:

1. Saves the result to `sessionStorage` under `theVoidDebrisResult`.
2. Saves the best result to `localStorage` under `theVoidDebrisBest`.
3. Dispatches a browser event:

```js
window.addEventListener('thevoid:debris-complete', (event) => {
  console.log(event.detail);
});
```

4. When embedded in an iframe, posts:

```js
{
  type: 'THE_VOID_DEBRIS_COMPLETE',
  result: { /* performance data */ }
}
```

The **CONTINUE** button dispatches `thevoid:debris-continue` and posts `THE_VOID_DEBRIS_CONTINUE` with the same result object.

The result includes star rating, distance, endpoint status, time remaining, meteorites obliterated, obliteration score, overall score, impacts and engine outages.

## Difficulty tuning

The main values are grouped in `CONFIG` at the top of `game.js`, including meteor health, points, spawn weighting, defence recovery, engine outage length and star thresholds.
