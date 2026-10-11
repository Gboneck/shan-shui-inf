# {Shan, Shui}*
Procedurally-generated vector-format infinitely-scrolling Chinese landscape for the browser.
Generate your own on https://lingdong-.github.io/shan-shui-inf/ (or [Alternative link](https://shan-shui-inf.glitch.me)).

Some examples:
![Screenshot1](/screenshots/screen001.jpg?raw=true "")
![Screenshot2](/screenshots/screen002.jpg?raw=true "")

{Shan, Shui}\* is inspired by [traditional Chinese landscape scrolls](https://en.wikipedia.org/wiki/Shan_shui) (such as [this](https://en.wikipedia.org/wiki/Dwelling_in_the_Fuchun_Mountains) and [this](https://en.wikipedia.org/wiki/Wang_Ximeng)) and uses noises and mathematical functions to model the mountains and trees from scratch. It is written entirely in javascript and outputs Scalable Vector Graphics (SVG) format.

## This edition

This fork keeps the original generator and changes how the scroll is shown and travelled, then widens what it paints.

![Blue-green, light crimson, snow and moonlight styles](/screenshots/styles.jpg?raw=true "")

- **A scroll that keeps moving.** The painting fills the window. Drag it, scroll it, hold the arrow keys, or press <kbd>Space</kbd> (or ▶) to let it drift by on its own at an adjustable speed.
- **Painted in the background.** The generator runs in a Web Worker and the page adds each mountain as it arrives, back to front, so the first strokes show in under a second and the page stays responsive while new country is painted ahead of you. Far-away parts are let go and repainted when you come back.
- **Three traditions.** The same mountains, painted by three schools:
  - 山水 **Shan shui**, the original Chinese ink landscape, with pavilions, pagodas and fishing boats.
  - 浮世絵 **Ukiyo-e**, after Hokusai's and Hiroshige's woodblock prints: Prussian-blue keylines, Mount Fuji behind the far hills, torii gates, thatched minka, five-storey pagodas, drum bridges, mat-sailed ferries and banded suyari-gasumi mist.
  - **Romantic**, after English watercolourists such as Turner, Girtin and Cotman: sepia line and washes that cool with distance, cottages, parish churches, castles and ruined abbeys, windmills, stone bridges and sailing boats.
- **Styles for each.** Shan shui: 水墨 Ink (the original), 青綠 Blue-green after Wang Ximeng's mineral colours, 淺絳 Light crimson, 雪景 Snow with falling flakes and 月夜 Moonlight. Ukiyo-e: 藍摺 Blue print, 赤富士 Red Fuji, 雪 Snow and 夜 Night. Romantic: Watercolour, Golden hour, Sepia, Winter and Nocturne. Switching style is instant; switching tradition repaints.
- **A journey through the seasons.** Every few minutes of travel the year turns: blossom on bare branches in spring, full leaf in summer, red and gold ridges in autumn, and in winter a grey wash over sky and water so the mountains stand out as snow. Can be turned off.
- **Mist, waterfalls and bridges.** Soft mist lies across the feet of the middle-distance ranges, ribbons of white water fall down some of the tallest peaks, and small bridges with travellers cross creeks on the plateaus.
- **Poems.** Every few screens, where the sky is open, a poem is written in, chosen to fit the season: a Tang or Song couplet in two columns with the poet's name and red seals, a haiku by Bashō, Buson, Issa, Chiyo-ni or Shiki in a print's title cartouche, or two lines of English verse by Wordsworth, Keats, Shelley, Byron and others.
- **Birds.** Small flocks cross the sky, more of them in spring and autumn.
- **Ambient mode.** When idle, the controls fade away, the scroll drifts and the screen stays awake, for leaving it on as a screensaver.
- **Sound.** ♪ or <kbd>M</kbd> turns on generated music played by the landscape itself: as mountains, houses, boats and poems pass the centre of the view they sound notes on a guqin, a koto or a harp (by tradition) over a bed of water and wind that changes with the season. Everything is synthesised in the browser; there are no audio files.
- **Same place, same view.** Each part of the world is generated from the seed, the tradition and its own position, so a place looks the same however you got there and whatever the window size. The noise field is seeded exactly as before, so an old seed keeps its mountain ranges, though the details differ.
- **Links that remember where you are.** The address updates as you travel: `?seed=…&x=…&trad=…&style=…`. Seeds can be any text, including Chinese.
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
| <kbd>M</kbd> | Sound on or off |

### URL parameters

| Parameter | Meaning |
| --- | --- |
| `seed` | Any text. Same seed, same world. |
| `x` | Position along the scroll, in world units. |
| `trad` | `shanshui` (default), `ukiyoe` or `romantic`. |
| `style` | Shan shui: `ink`, `qinglu`, `qianjiang`, `snow`, `night`. Ukiyo-e: `aizuri`, `akafuji`, `yuki`, `night`. Romantic: `watercolour`, `golden`, `sepia`, `snow`, `night`. Without it the page follows your system's light or dark setting until you pick one. |
| `seasons` | `0` paints an endless summer instead of the turning year. |
| `debug` | Exposes `window.shanshui` for poking at from the console. |

### How the page is put together

Everything is still one `index.html` with no build step. The original generator sits in `<script type="text/x-shanshui">` blocks, followed by the Japanese and European drawing code and the planner: the page doesn't run them directly but joins them into a Web Worker (or runs them inline if workers are unavailable). The worker plans the world in 512-unit columns, the same width as the original chunks, and renders one item at a time on request.

The original drawing code is unchanged apart from a few `HOOK` calls, through which the planner swaps in each tradition's buildings, boats and leaf colours and adds waterfalls, bridges and blossom. Those extras draw from their own random streams, so turning them on or off never changes the rest of the picture. Each tradition recolours the original's grey ink (Prussian blue for woodblock, sepia for watercolour), and the styles add colour through CSS classes on mountain bodies, walls, roofs and sails, so switching style never repaints.

The sound engine (`Sound`, in its own script block) is plain Web Audio: Karplus–Strong plucked strings, a generated reverb, filtered-noise water and wind, and an Aeolian harp, with events rate-limited onto a slow swung pulse.

On the page, the painting lives in a wide strip that a hidden-scrollbar container scrolls across. Browsers move a scroll offset without repainting, while moving this much SVG with a CSS transform made Chrome re-layer it every frame. Paper, sky and ink share that strip so the ink can still multiply onto the paper.
