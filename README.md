# {Shan, Shui}*
Procedurally-generated vector-format infinitely-scrolling Chinese landscape for the browser.
Generate your own on https://lingdong-.github.io/shan-shui-inf/ (or [Alternative link](https://shan-shui-inf.glitch.me)).

Some examples:
![Screenshot1](/screenshots/screen001.jpg?raw=true "")
![Screenshot2](/screenshots/screen002.jpg?raw=true "")

{Shan, Shui}\* is inspired by [traditional Chinese landscape scrolls](https://en.wikipedia.org/wiki/Shan_shui) (such as [this](https://en.wikipedia.org/wiki/Dwelling_in_the_Fuchun_Mountains) and [this](https://en.wikipedia.org/wiki/Wang_Ximeng)) and uses noises and mathematical functions to model the mountains and trees from scratch. It is written entirely in javascript and outputs Scalable Vector Graphics (SVG) format.

## This edition

This fork keeps the original generator and changes how the scroll is shown and travelled.

![Blue-green, light crimson, snow and moonlight styles](/screenshots/styles.jpg?raw=true "")

- **A scroll that keeps moving.** The painting fills the window. Drag it, scroll it, hold the arrow keys, or press <kbd>Space</kbd> (or ▶) to let it drift by on its own at an adjustable speed.
- **Painted in the background.** The generator runs in a Web Worker and the page adds each mountain as it arrives, back to front, so the first strokes show in under a second and the page stays responsive while new country is painted ahead of you. Far-away parts are let go and repainted when you come back.
- **Five styles.** 水墨 Ink (the original), 青綠 Blue-green after Wang Ximeng's mineral colours, 淺絳 Light crimson with umber and pale indigo washes, 雪景 Snow with falling flakes, and 月夜 Moonlight. Switching is instant and doesn't regenerate anything.
- **Inscriptions and seals.** Every few screens, where the sky is open, a couplet from a Tang or Song poet is written in, with the poet's name and one or two red seals.
- **Birds.** Small flocks cross the sky.
- **Same place, same view.** Each part of the world is generated from the seed and its own position, so a place looks the same however you got there and whatever the window size. The noise field is seeded exactly as before, so an old seed keeps its mountain ranges, though the details differ.
- **Links that remember where you are.** The address updates as you travel: `?seed=…&x=…&style=…`. Seeds can be any text, including Chinese.
- **Save.** *Snapshot PNG* renders the current view at up to 2× resolution. *Download SVG* saves the current view as a standalone vector file.
- **Phones.** On a narrow screen the painting sits in a band across the middle, like a scroll laid on a table, and travels by touch.

### Controls

| Input | Action |
| --- | --- |
| Drag, mouse wheel, trackpad | Travel along the scroll |
| <kbd>←</kbd> <kbd>→</kbd> | Travel while held |
| <kbd>Page Up</kbd> <kbd>Page Down</kbd> | Jump most of a screen |
| <kbd>Space</kbd> | Drift on or off |
| <kbd>Home</kbd> | Back to the start |
| <kbd>F</kbd> | Full screen |

### URL parameters

| Parameter | Meaning |
| --- | --- |
| `seed` | Any text. Same seed, same world. |
| `x` | Position along the scroll, in world units. |
| `style` | `ink`, `qinglu`, `qianjiang`, `snow` or `night`. Without it the page follows your system's light or dark setting until you pick one. |
| `debug` | Exposes `window.shanshui` for poking at from the console. |

### How the page is put together

Everything is still one `index.html` with no build step. The original generator sits in `<script type="text/x-shanshui">` blocks: the page doesn't run them directly but joins them into a Web Worker (or runs them inline if workers are unavailable). The worker plans the world in 512-unit columns, the same width as the original chunks, and renders one item at a time on request.

On the page, the painting lives in a wide strip that a hidden-scrollbar container scrolls across. Browsers move a scroll offset without repainting, while moving this much SVG with a CSS transform made Chrome re-layer it every frame. Paper, sky and ink share that strip so the ink can still multiply onto the paper.
