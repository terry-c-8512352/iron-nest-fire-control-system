# Iron Nest — Fire Control System

**▶ Use it now: <https://terry-c-8512352.github.io/iron-nest-fire-control-system/>**

A fast, portable fire-control calculator to run **alongside** *Iron Nest: Heavy Turret
Simulator*. Type your gun grid and the target grid; it instantly gives **range,
bearing, and every valid powder-charge + gun-elevation option**. It also solves
**moving-target intercepts** (aim point, bearing, elevation, time-to-impact).

It's a single self-contained `index.html` — **no install, no build, no internet**. Open
it in any browser (desktop, phone, tablet). Everything runs locally; calibration and
settings are saved in the browser.

## Run it

Easiest: open the hosted copy at
<https://terry-c-8512352.github.io/iron-nest-fire-control-system/>. Nothing to download.

Or run it locally — open `index.html` in a browser, double-click it, or drag it into a
browser tab. Works fully offline. On a phone, use *Add to Home Screen* for one-tap access.

Either way everything runs in your browser: your settings, calibration and saved
references are stored on your own device and are never shared or uploaded.

## Picking positions — the map (fastest)

Instead of typing coordinates, use the map at the top of **Positions**:

- **Tap** an empty spot to drop the **Gun** — it then auto-switches to **Target**, so a
  second tap places the target. One-two and you have a solution.
- **Drag** either marker to nudge it; **tap a marker** to re-select it for placing.
- **Wheel / pinch** to zoom (the 100 m subgrid appears when you're zoomed in), **drag**
  empty space to pan, **Fit** returns to the whole map.
- Use the **Place Gun / Place Target** buttons to choose which one the next tap sets.

Everything snaps to the 100 m subgrid and stays in sync with the text fields below — so
you can tap to get close, then fine-tune by typing, or vice-versa. The dashed line shows
range and bearing live.

## Position format (typing, still supported)

```
<Col><Row>-<subX>,<subY>
```

- **Col** — map column, `A`–`T`
- **Row** — map row, `0`–`10`
- **subX,subY** — subgrid cell inside that 1 km square, `0`–`9` (each = 100 m)
- Decimals allowed for finer aim, e.g. `F5-3.5,4.2`

Examples: `F5-5,5`, `K7-1,8`, `A0-0,0`.

## Two targets (two shells from one gun)

Tick **Second target** in Positions to add **Target B**. You then get a firing solution
block for the **one gun → each target** — Target A and Target B — so you can lay in two
shots at two targets back-to-back. The gun is blue; Target A is orange, Target B purple,
each with its own coloured line and live range/bearing on the map.

- The map's place selector is **Gun / Target A / Target B**; tapping empty space cycles
  Gun → Target A → Target B, so three taps sets the whole two-shot problem.
- The moving-target intercept still works from the single gun (its speed+heading mode has
  its own Sighting position field, falling back to Target A when left blank).
- Untick Second target to go back to a single target.

## Saved references

Saved references live in the **left-hand rail**, always on screen as a stack of cards —
handy for known targets (HQ, bridges, chokepoints).

- Type a name and hit **Save**; it snapshots the gun, Target A and Target B (if enabled).
- You can keep **6** at a time — the header shows how many slots are used (e.g. `4/6`).
  Saving a 7th drops the **oldest** automatically, so the rail stays a short list of what
  you're actually shooting at. Delete one with **✕** to free a slot deliberately.
- Each card shows the target grid and its **range, bearing and elevation** as three
  read-at-a-glance figures, plus the recommended charge — so a known target is a glance,
  not a re-entry.
- A two-target reference gets **two** stat rows on the one card, labelled **A** and **B**.
- **Load** drops the positions straight back onto the map (re-enabling the second target
  if the reference had one) and recomputes. **✕** deletes it.
- Cards follow your **Settings** — switch bearing to mils and they update with everything
  else.
- References persist in the browser between sessions.

On narrow screens (phones) the rail drops below the main column instead of sitting beside
it, so nothing is squeezed.

## The ballistics (from the in-game Firing Table)

Every powder charge is a straight line — **elevation° = (12 ÷ charge) × range(km)** —
capped at the gun's max **60°**, with each charge covering a 5 km band:

| Charge | Range band | Elevation at top of band |
|:------:|:----------:|:------------------------:|
| 1 | 0–5 km   | 60° |
| 2 | 5–10 km  | 60° |
| 3 | 10–15 km | 60° |
| 4 | 15–20 km | 60° |
| 5 | 20–25 km | 60° |
| 6 | 25–30 km | 60° |

For any range, the app shows **all** charges that can reach it and flags the **minimum
charge** (steepest arc, least propellant) as recommended — higher charges give flatter,
faster shots. Max range is **30 km**. This is exact, so no elevation calibration is
needed. (The table's per-0.01 km "AP correction" is just linear interpolation, which the
formula already does.)

## Moving-target intercept

Enable **Moving Target**. There are two ways to describe the target's motion, and both
feed the same predictions:

### Two sightings (recommended)

Enter two spotter reports — *"was at **P1** at time **T1**, then at **P2** at time
**T2**"*. The app derives the target's speed and heading for you, then:

- **Predicts** where it will be: type any time in *"Predict position at time"* and it
  shows the grid cell plus range/bearing from your gun at that moment.
- **Solves the intercept**: per charge, the aim-point grid, bearing, elevation, flight
  time, and the **impact time** ("fire now, shell meets target at 12:00:50").

Times can be plain **seconds** (`0`, `30`) or a **clock** (`12:30:30` / `mm:ss`). If you
use clock times, the predicted and impact times come back as clock times too. The two
sighting times must differ; order doesn't matter (it uses the later one as the anchor).

### Speed + heading

If you already know the target's course, switch to this mode and enter four things:

- **Sighting position** — where you saw it. Leave blank to use **Target A** from the map.
- **First seen at** — plain seconds or a clock (`07:40:00`).
- **Heading** — the compass course it's holding.
- **Speed** — with a unit picker: **knots** (default), km/h, mph or m/s. So `19.4` and
  *knots* is what you'd read off the game. Your choice is remembered between sessions.

### Predicted track — when to fire

Below the intercept table is a schedule of where the target will be, stepping along its
course in fixed distance intervals — **0.5 km × 10 waypoints** by default, both adjustable.

For every waypoint you get the arrival time, grid position, range, bearing, elevation, the
recommended charge (alternatives in brackets), and — the useful bit — the **time to fire**:

```
Fire at  =  arrival time  −  shell flight time
```

So you can lay the gun ahead of time and pull the trigger on the clock, instead of trying
to solve a lead in your head. Rows that can't be shot are called out rather than quietly
given wrong numbers: **out of range** past the gun's 30 km, **off map** once the track
leaves the grid, and **too late** when even the fastest shell can't get there in time.

This works from *either* input mode — two sightings or speed + heading — since both end up
describing the same track.

**Click any predicted position** — an aim point in the intercept table, or a waypoint in
the track table — to save it straight to the reference rail as a card, named for what it
was (`Aim C4 07:40:22`, `Track 1.5km 07:42:30`). The card carries its own range, bearing
and elevation from your current gun, and **Load** puts it back on the map. Positions that
have run off the grid aren't offered, and the usual 6-card cap applies.

The course is also **plotted on the map**: a pencilled track line from the sighting, with a
fix mark and distance label at every waypoint and an arrowhead showing which way it's
running. **Filled** marks are still shootable; **hollow** ones are out of range or too
late — so the stretch you can actually hit reads at a glance. The plot is clipped to the
paper, so a track running off the grid stops at the sheet edge.

### Fire-at time

Leave *"Fire at time"* blank to fire **now** (at the latest sighting / observation).
Set it to fire later — the solver advances the target to that moment first, so the aim
point and impact time account for the delay.

## Flight times (measured)

The in-game firing table has no time-of-flight data, so it was timed by hand with the
game's stopwatch — four shots per charge, 24 samples in `times.csv`. The result is simple:

> **Flight time is proportional to range, with a constant per charge.**
> No intercept term — a zero-range shot takes zero seconds.

| Charge | s/km | Flight time at that charge's max range |
|:--:|:--:|:--|
| 1 | 4.81 | 24.1 s @ 5 km |
| 2 | 3.87 | 38.7 s @ 10 km |
| 3 | 2.61 | 39.1 s @ 15 km |
| 4 | 1.90 | 38.0 s @ 20 km |
| 5 | 1.54 | 38.5 s @ 25 km |
| 6 | 1.43 | 43.0 s @ 30 km |

The fit is tight: charge 4 reproduces all four of its samples **exactly**, and charges 3, 5
and 6 land within 0.2 s. Note that **elevation alone doesn't determine flight time** — every
charge reaches 60° at its maximum range, yet those shots take anywhere from 24 s to 43 s.
It's the charge *and* the range.

These figures ship in the app, so nothing needs calibrating and no `EST` tags appear. If the
game ever changes, open **Time-of-flight calibration** and log **charge · range · seconds**;
**two or more** points for a charge override the built-in figure with your own fit. A single
point is kept but not used — the table is fitted from four samples per charge, so one
hand-timed shot shouldn't outrank it. Overrides save locally and persist between sessions.

## One-time setup: check your north axis

Bearing depends on which way the map's rows run. The default is **north-up**: higher row
number is North, so **row 10 is at the top** of the map and row 0 at the bottom (matching
the in-game map, e.g. A10 top, A0 bottom). The on-screen map is drawn the same way. Verify
once:

- Put a target due **East** of your gun (same row, higher column). Bearing should read
  **90°**; a target one row **higher** (further up the map) should read **0° / North**.
  If North/South looks mirrored, open **Settings** and untick the north-up option — that
  flips both the bearings and the map together.

Settings also has an angle-unit display (degrees / 6400 NATO mils / 6000 mils) for
bearing; elevation always shows in **degrees** to match the firing table.

## Verifying / self-tests

Open **Self-tests** in the app and click **Run self-tests** — it checks the geometry and
ballistics against known firing-table rows (e.g. charge 3 @ 10.1 km = 40.40°) and the
intercept solver. All green means the math matches the table.

## Files

- `index.html` — the entire app (HTML + CSS + JS, no dependencies).
- `272geqmnqrih1.jpeg` — the in-game Firing Table this is built from (reference).
- `README.md` — this file.
