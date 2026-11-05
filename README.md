# arjunprabhakaran.com

A single static page. No build step, no dependencies — open `index.html` or drop the
folder on any host (GitHub Pages, Netlify, Vercel, S3).

```
index.html    the page, plus the drag handler and lightbox
style.css     every styling decision lives here
photos/       the charm photographs (WebP, plus the Focalect logo PNG)
```

## Photos

| file | where it appears |
|---|---|
| `focalect-dashboard.webp` | hanging charm — "focalect" |
| `frc-windy-city.webp` | hanging charm — "windy city" |
| `continuum-team.webp` | hanging charm — "continuum", and the Continuum entry |
| `focalect-logo.png` | Focalect entry charm |
| `imts.webp` | FIRST Robotics entry charm |
| `frc-team.webp` | **unused** — swap it in anywhere you prefer it |

All re-encoded to WebP at 1200–1400px, ~450 KB for the set. That is sized for the
lightbox; the tags themselves render at 76–112px.

The CNN paper entry has **no charm**, because there is no photograph of it. An entry
without one simply lets its text run the full width — that is intentional, not a
missing file. A figure from the paper would drop straight in.

Each `--ar` is set to match its photo so `object-fit: cover` crops as little as
possible: `16/10` for the dashboard, `3/4` for the portrait banner, `4/3` for the
Continuum photo. Photos display in their own colours, so they want to be reasonably
consistent with each other — the page does nothing to harmonise them.

## Images: keychains

Photos hang off a hairline rail as little tags, each punched with a hole and hung
from a ring on its own string.

**Photographs, untouched.** The images carry no colour treatment — what you drop in
is what shows. An earlier version ran them through an SVG duotone that mapped them
onto the navy-to-pink ramp; it looked good on abstract placeholder art and turned real
faces and screenshots into an unreadable pink mass, so it is gone. The palette is
carried by the hairlines instead: the rail, strings, rings, bullets and underlines are
all `#FF007F`, and the photographs sit inside that frame in their own colours.

**A real hole.** The tag's `mask-image` punches a transparent disc near the top and
the ring sits in it, so the page shows *through* the artwork rather than a circle
being drawn on top of it.

**Their own clocks.** Each charm has a different `--period`, `--phase`, `--amp` and
string `--len`, so the cluster drifts in and out of sync. Pointing at one speeds it up.

**Grab or click.** A pointer handler takes over from the CSS animation, follows the
cursor, then releases into a damped pendulum (`vel += -angle * 0.014`, `vel *= 0.945`)
until it settles and hands back to the idle sway. Grabbing adopts the charm's current
angle rather than snapping, and both the angle and the throw velocity are clamped so a
fast flick can't send it spinning.

Note the sign in `angleTo()`. With `transform-origin: 50% 0`, CSS `rotate(θ)` sends the
hanging point `(0, L)` to `(−L·sinθ, L·cosθ)` — so a *positive* angle swings the tag
*left*, while `atan2(dx, dy)` is positive when the cursor is to the *right*. The result
is negated to match. Remove that minus sign and the charms run away from the pointer.

A press that moves less than 4px is treated as a click, not a drag, and opens the photo
full size in a lightbox — undyed, so you see the real colours. Escape or the backdrop
closes it, focus moves to the close button and returns to the charm afterwards, and the
charms are `role="button"` with `tabindex="0"` so Enter and Space work too. The 4px slop
is what keeps an ordinary drag from also firing the lightbox.

To hang a new one, copy a `<figure class="charm">` and vary all four knobs — matching
values make the cluster look mechanical.

## Palette

Navy carries the type, hot pink carries every line.

| token | light | dark | role |
|---|---|---|---|
| `--paper` | `#FAF8FB` | `#0F1056` | page |
| `--ink` | `#15306A` | `#DDE2F4` | body text |
| `--heading` | `#0F1056` | `#FFFFFF` | headings, entry titles |
| `--muted` | `#5C6689` | `#9298BD` | years, labels, captions |
| `--rule` | `#FF007F` | `#FF4D9E` | bullets, underlines, rails, rings, +/– markers — every line on the page |
| `--pink-deep` | `#C70063` | `#FF8AC0` | link underline on hover |
| `--mark` | `#FFE6F2` | `#4A0B38` | link highlight on hover |

**Pink is never used for text.** Link *text* stays navy; only the underline is pink.
That keeps every reading surface above 4.5:1 while the pink does the accent work at
3:1+, which is the threshold for non-text elements.

Measured against the paper: body 11.96:1, headings 16.18:1, muted 5.35:1, pink rules
3.58:1 (light); 13.23 / 17.09 / 6.06 / 5.54 (dark).

## Layout notes

- One 672px column, centred. No nav, no hero, no cards.
- Built for scanning: 16px body, 15px page bullets, 14px inside work entries. Prose
  paragraphs are capped at `66ch` so lines land around 70 characters; `.role` opts out
  with `max-width: none` because it's a standing line, not prose.
- Each headline entry is a one-line lead plus two to four bullets, never a block of
  prose. **What I'm building toward** is the deliberate exception — the one piece of
  sustained prose on the page — and it's kept to two short paragraphs.
- **Tools** reuses the `.elsewhere` two-column `<dl>` — same component, grouped by
  ML / Web / Data / Hardware.
- Section headings are quiet uppercase labels, not titles — hierarchy comes from
  spacing, not size.
- Work is deliberately two-tier. **Selected work** holds the four that matter —
  Focalect, FIRST Robotics, the CNN paper and Continuum — each a three-part row with
  the year in a left gutter, title and full note in the middle, and a charm on the
  right. **Other work** is four native `<details>` elements: year, title and a short
  hint are always visible, the rest unfolds on click. No JavaScript, and it stays
  keyboard-accessible and findable by in-page search.
- Entries without a URL are a `<div>`, not an `<a>`, so they don't look clickable;
  titles that do link are ordinary `<a>` elements, which keeps anchors from nesting.
- The `<summary>` is a grid, not flex — on narrow screens it re-flows to
  `"year title mark" / "hint hint hint"` so the +/– marker stays beside the title
  instead of dropping to its own line.
- Contact is a two-column `<dl>`, not a row of pipe-separated links.
- `--i` on each `.stagger` block sets the entrance order; they fade up 65ms apart.
  Renumber if you move sections.

## The through-line

The opening three paragraphs point at physical AI — the role line, the hero sentence
and the last clause of the bio. That's deliberate and deliberately light: the work
entries stay factual, and **What I'm building toward** carries the actual thesis
(vision robots that read customer behaviour from movement rather than faces). If you
want to lean harder into it, that section is the place, not the entries.

## Kept off the page, deliberately

No home address, phone number, birthdate, citizenship or immigration details, no
family members' names or contact details, no financial-aid information. No family
member is referred to at all; "a family business" is unnamed beyond BuySide Global
itself.

## Local preview

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```
