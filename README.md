# Physics Sandbox | A 2D Physics Playground

Build, drop, throw and connect things in a 2D physics playground. Gravity, collisions, friction, springs and pendulums, powered by a rigid-body engine written for this page. No libraries.

Everything lives in one file, `index.html`. No build step, no dependencies, no folders.

## Put it on GitHub Pages

1. Open your `PhysicsSandbox` repository on GitHub.
2. Choose **Add file > Upload files** and drag in `index.html`. It replaces your current `index.html` because the name is the same.
3. Commit. After about a minute, hard-refresh your page (Ctrl+Shift+R).

Your old files (a separate stylesheet or script) are no longer used. You can leave them or delete them.

## How to play

- **Drag** (V): grab an object and throw it. Click one to inspect it (edit its mass, bounciness and friction, or make it fixed). Drag empty space to pan.
- **Circle, Box, Triangle, Hexagon, Plank** (C, B, T, H, P): click to drop one, or click and drag back to launch it. A preview shows the speed and the arc it will fly.
- **Wall** (W): drag to draw a fixed wall or ramp.
- **Rod, Rope, Spring** (R, O, S): click two points to join them. Click empty space to anchor to the world.
- **Hinge** (J): click a pivot, then the other body (or empty space to pin to the world).
- **Attractor** (A): click to place a gravity well. Click it again to remove it.
- **Erase** (X): click an object or a joint to delete it.
- **Pause / Play** (Space), **Step** (.), **Fit** (F), zoom (Ctrl + scroll). Toggles for velocity vectors, trails, contact points and the arena walls.
- **Scenes**: Pyramid, Dominoes, Newton's cradle, Pendulum chain, Wrecking ball, Bridge.
- **World settings**: gravity (with Moon, Mars, Earth, Jupiter and Zero-g presets), time scale, and the bounciness, friction and size of new objects.
- **Energy** section: live charts of kinetic, potential and total energy, and total momentum.

## The engine

Circles and convex polygons, with:

- semi-implicit Euler integration, split into at least 4 sub-steps per frame
- collision detection: circle distance, plus the separating axis test with edge clipping for polygons (up to two contact points)
- a sequential-impulse contact solver with restitution, Coulomb friction and remembered ("warm start") impulses so stacks stay still
- joints: hinge, rod, rope (pulls only) and spring (a soft constraint with a frequency and damping ratio)

I checked it against exact results:

- free fall, and the bounce height law `h' = e^2 h`
- elastic collisions: equal masses swap velocities, and a 1:3 mass collision gives exactly -2 and +2 m/s from 4 m/s
- friction on an incline: a box slides at `g (sin t - mu cos t)` and sticks when `mu > tan t`
- a disc rolling without slipping at `(2/3) g sin t`
- a pendulum period within 0.1% of `2 pi sqrt(L/g)`, and a spring at exactly its set frequency
- linear and angular momentum conserved to about 1e-13 in an off-axis box collision
- a stack of 8 boxes, a 36-box pyramid and a bridge staying put, and Newton's cradle behaving like the real thing

## Limits

2D only. No sleeping. No continuous collision detection, so very fast thin objects can tunnel through walls. Spring energy is not counted in the energy readouts. Not intended for engineering analysis.

## Tweak it

- Scenes: the `SCENES` array. Each one is a small function that adds bodies and joints.
- World size: `WORLD_W` (metres).
- Solver quality: `ITER`, `BETA` and `SLOP` at the top of the `Phys` module.
- Source link in the header: the `pill` link in `index.html`.
- In the browser console, `SandboxLab.world()` gives you the live world, for experiments.
