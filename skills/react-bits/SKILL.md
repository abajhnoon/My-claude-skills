---
name: "react-bits"
description: "Use React Bits animated React components (text animations, backgrounds, effects, UI) in a React or Next.js website, pulled from a pinned, security-reviewed GitHub commit."
---

# React Bits (free library)

218 animated React components in TypeScript + Tailwind, from github.com/DavidHDev/react-bits. Use them only in React / Next.js projects. For plain HTML sites, port the effect by hand instead.

Categories: `TextAnimations` (33), `Animations` (38), `Backgrounds` (64), `Components` (48), `Micro` (35).

## Project stack rule: GSAP is the only JS animation library

The user's stack is GSAP + Lenis for all motion. Do not add the `motion` (Framer Motion) package to a project. The following 28 components import `motion/react`, so they are **excluded**. Do not copy them, and never install `motion` to make one work:

- `Components/Stack`
- `TextAnimations/`: `BlurText`, `CircularText`, `CountUp`, `DecryptedText`, `RotatingText`, `ScrollVelocity`, `TrueFocus`
- `Micro/`: `BellToggle`, `CodeSlots`, `CometDial`, `DodgeField`, `FlipCard`, `JellyRadio`, `PromptBar`, `RubberSegment`, `ScrubField`, `SlideCommit`, `SlingButton`, `SpringCheck`, `SquishSwitch`, `StatusMark`, `SwipeRow`, `SwipeToast`, `TearTicket`, `ThoughtLine`, `WakeSlider`, `WarmTooltip`

Also excluded: `Backgrounds/GridScan` (loads face-api.js and face models from a CDN for face tracking; a privacy and performance problem for a corporate site).

Before copying any component, still check its imports: if it imports `motion/react` (for example after an upgrade), treat it as excluded. If the user needs an excluded effect (e.g. a count-up number or blur-in text), rebuild it with GSAP following the gsap-core / gsap-react skills.

Allowed dependencies: `gsap` / `@gsap/react`, `ogl`, `three` (+ `@react-three/*` only if already used), `matter-js`, `@use-gesture/react`, `@hugeicons/react` + `@hugeicons/core-free-icons`.

## Setup (once per session)

The reactbits.dev registry may be unreachable from the workspace, so fetch the source from GitHub, pinned to commit `d965399df489c7b8da3874e07d716c0d98ab39fd` (the reviewed version). Never fetch `main` or a newer commit unless the user asks to re-review and upgrade.

```bash
SHA=d965399df489c7b8da3874e07d716c0d98ab39fd
D="$HOME/.cache/react-bits/$SHA"
if [ ! -d "$D/src/ts-tailwind" ]; then
  rm -rf "$D" && mkdir -p "$D" && cd "$D" && git init -q \
  && git remote add origin https://github.com/DavidHDev/react-bits.git \
  && git -c core.hooksPath=/dev/null fetch -q --depth 1 --filter=blob:none origin "$SHA" \
  && git sparse-checkout set --no-cone '/src/ts-tailwind/' '/LICENSE.md' \
  && git -c core.hooksPath=/dev/null checkout -q FETCH_HEAD
fi
ls "$D/src/ts-tailwind"
```

Components live at `$HOME/.cache/react-bits/d965399df489c7b8da3874e07d716c0d98ab39fd/src/ts-tailwind/<Category>/<Name>/<Name>.tsx`. List a category folder to browse names. Never execute anything from this folder; only read and copy files.

If the reactbits.dev registry is reachable in the project environment, `npx shadcn@latest add @react-bits/<Name>-TS-TW` installs the same component (the exclusion list still applies).

## Using a component

1. Read the `.tsx` file fully before copying. Confirm it is not on the excluded list and imports nothing outside the allowed dependencies.
2. Copy it into the project (e.g. `components/react-bits/<Name>.tsx`).
3. Install only the npm dependencies it imports.
4. In Next.js App Router, add `"use client"` at the top of any component using hooks, canvas or WebGL.
5. When it uses GSAP, follow the gsap-react and gsap-scrolltrigger skills (useGSAP, scope, cleanup). Never let a React Bits component and page-level GSAP code animate the same element.
6. Restyle it to the project's design tokens (colors, fonts, spacing, radii). Never ship a component with its demo colors.

## Mandatory review before shipping

- **Replace demo media.** Several components default to remote demo images (Unsplash, picsum, twimg). Swap in the project's own assets.
- **Self-host fonts.** Some components load Google Fonts by URL (`TextPressure`, `VariableProximity`, `CircularGallery`). Replace with the project's font setup.
- **`Micro/Shredder`** fetches the image `src` you pass it; give it local images only.
- **Smooth scroll.** Lenis is the only smooth-scroll layer. Remove any component-level smooth-scroll or `scroll-behavior: smooth`, and never add GSAP ScrollSmoother.
- **Reduced motion.** Wrap heavy effects so they are skipped or static when `prefers-reduced-motion: reduce` matches.
- **Performance.** WebGL backgrounds (`ogl`, `three`) are heavy: at most one per page, lazy-load below the fold, and a static fallback on low-power mobile.
- **RTL.** For Arabic pages (`dir="rtl"`), check that directional animations (slide-in, marquee, horizontal scroll) run the right way and mirror them where needed.

## License

MIT + Commons Clause: free to use inside any website or product, including commercial ones. Do not sell or redistribute the components themselves, alone or as a pack.