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

## Position format

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

Enable **Moving Target**, then enter the target's **speed (m/s)** and **heading (°)**.
The target's *current* spot is the Target field; the solver leads it and returns, per
charge, the aim grid, bearing, elevation, and time-to-impact.

⚠️ **Flight times need calibrating.** The firing table has no time-of-flight data, so
intercept timing starts as a rough **estimate** (marked `EST`). To make it accurate:

1. Fire a shot and count the seconds until impact.
2. Open **Time-of-flight calibration** and log **charge · range (km) · seconds**.
3. Two or more points for a charge → the app fits a line and drops the `EST` tag.

Calibration is saved locally and persists between sessions.

## One-time setup: check your north axis

Bearing depends on which way the map's rows run. The default assumes **row 0 is North**
(top of map), rows increasing southward. Verify once:

- Put a target due **East** of your gun (same row, higher column). Bearing should read
  **90°**. If North/South looks mirrored, open **Settings** and toggle the row-direction
  option, then re-check.

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
