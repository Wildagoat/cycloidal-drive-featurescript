# Cycloidal Drive FeatureScript

A custom [Onshape](https://www.onshape.com/) FeatureScript that generates a complete **cycloidal reducer disk** from first principles — the lobed epicycloid profile, the eccentric center bore, and the load-pin holes — as one parametric, fully-associative feature that rebuilds cleanly at any drive ratio.

A cycloidal disk is the heart of a cycloidal reducer, but its lobed profile is a true epicycloid: painful to draw by hand, and standard sketches don't regenerate correctly when the drive ratio or eccentricity changes. This feature treats the reducer geometry as equations rather than sketches.

## Parameters

| Input | Symbol | Role |
|-------|--------|------|
| Base circle diameter | `d` | Sets the disk pitch geometry |
| Rolling circle diameter | `delta` | With `d`, fixes the lobe count / reduction ratio (`ratio = d / delta`) |
| Eccentricity | `e` | Contact offset of the cycloidal motion |
| Load pin diameter | `dl` | Load-pin hole size (offset internally by `2·e`) |
| Hole count | `hc` | Number of load-pin holes |
| Load holes pitch diameter | `dp` | Pitch circle the load holes sit on |
| Center bore | `bore` | Eccentric input bore |
| Depth | `depth` | Extrusion thickness |

## How it works

1. **Epicycloid path** — sweeps 50 control points of the cycloid and fits a periodic spline:
   ```
   x = (d + delta)/2 · sin(phi) − e · sin(phi + ratio·phi)
   y = (d + delta)/2 · cos(phi) − e · cos(phi + ratio·phi)
   ```
2. **Center bore** — a concentric circle for the eccentric input shaft.
3. **Load-pin holes** — `hc` circles placed on the `dp` pitch circle; each hole is enlarged by `2·e` so the pins clear the disk's eccentric motion.
4. **Extrude** — the solved sketch region is extruded to `depth`, producing the finished disk in one feature.

Because the lobe count is derived as `d / delta` and every hole is placed parametrically, changing a single drive parameter regenerates the entire disk correctly.

## Usage

Copy the contents of [`cycloidal-drive.fs`](./cycloidal-drive.fs) into a Feature Studio in Onshape (built against the `onshape/std` version pinned at the top of the file), then add the **Cycloidal Path** custom feature to a Part Studio.

## License

MIT
