# Surviving the Cyber Resilience Act

**Reproducible, auditable ROS stacks** — ROSCon 2026, 24 September 2026.

A talk by Wolf Vollprecht and Ruben Arts, [prefix.dev](https://prefix.dev), on what the EU Cyber Resilience Act means for robotics manufacturers: support periods, SBOMs, VEX, and the state of ROS packaging.

- [View the slides](https://prefix-dev.github.io/roscon-2026-cra-talk/)
- [Download the PDF](https://github.com/prefix-dev/roscon-2026-cra-talk/releases/latest)

## Run locally

Install [Pixi](https://pixi.sh), then run:

```sh
pixi run start
```

This installs Node.js, pnpm, and the presentation dependencies, then opens the deck at <http://localhost:3030>.

Edit [`slides.md`](slides.md) to update the presentation. The deck uses [Slidev](https://sli.dev) with a local Tufte-inspired theme in [`slidev-theme-tufte/`](slidev-theme-tufte/).

## Build

```sh
pixi run build  # static site in dist/
```

Pushes to `main` automatically build and deploy the slides to GitHub Pages. The PDF from the talk is attached to the release.
