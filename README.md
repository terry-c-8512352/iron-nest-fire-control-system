# Iron Nest — Fire Control System

A fast, portable fire-control calculator to run **alongside** *Iron Nest: Heavy Turret
Simulator*. Type your gun grid and the target grid; it instantly gives **range,
bearing, and every valid powder-charge + gun-elevation option**. It also solves
**moving-target intercepts** (aim point, bearing, elevation, time-to-impact).

It's a single self-contained `index.html` — **no install, no build, no internet**. Open
it in any browser (desktop, phone, tablet). Everything runs locally; calibration and
settings are saved in the browser.

## Run it

Just open `index.html` in a browser — double-click it, or drag it into a browser tab.
Works fully offline. On a phone, use *Add to Home Screen* for one-tap access.

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

Enable **Moving Target**. There are two ways to describe the target's motion:

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

If you already know the target's **speed (m/s)** and **heading (°)**, switch to this mode.
The target's current spot is the top **Target** field, observed at the "Observed at time"
you give. Everything else (predict, intercept, impact time) works the same.

### Fire-at time

Leave *"Fire at time"* blank to fire **now** (at the latest sighting / observation).
Set it to fire later — the solver advances the target to that moment first, so the aim
point and impact time account for the delay.

⚠️ **Flight times need calibrating.** The firing table has no time-of-flight data, so
intercept timing starts as a rough **estimate** (marked `EST`). To make it accurate:

1. Fire a shot and count the seconds until impact.
2. Open **Time-of-flight calibration** and log **charge · range (km) · seconds**.
3. Two or more points for a charge → the app fits a line and drops the `EST` tag.

Calibration is saved locally and persists between sessions.

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
