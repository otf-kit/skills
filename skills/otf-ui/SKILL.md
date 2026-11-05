---
name: otf-ui
description: >-
  Build accessible, production-ready web UI with @otfdashkit/ui — a free,
  open-source (MIT) React component library on Radix + Tailwind CSS v3. Use when adding
  buttons, forms, dialogs, sheets, tables, charts, command palettes, toasts, and
  more to a React / Next.js / Vite app, theming them, or installing components by
  npm or copy-paste. Component names match @otfdashkit/ui-native; props differ per platform.
license: MIT
metadata:
  source: otf-kit.dev
  package: "@otfdashkit/ui"
  npm: https://www.npmjs.com/package/@otfdashkit/ui
  repo: github.com/otf-kit/sdk
---

# @otfdashkit/ui — web components

A free, MIT React component library built on [Radix](https://www.radix-ui.com/) +
[Tailwind CSS v3](https://v3.tailwindcss.com/). Accessible (keyboard + screen-reader)
by default, themed via shared design tokens, and API-matched to
`@otfdashkit/ui-native` so the same component names work on native (props differ per platform).

## When to use this skill

Activate when adding or theming UI in a React / Next.js / Vite web app — buttons,
inputs, forms, dialogs, sheets, drawers, popovers, tables, charts, command palette,
toasts, calendars, and more. Browse the full set live (links below) rather than
guessing a component name.

## Install

```bash
pnpm add @otfdashkit/ui @otfdashkit/tokens
# npm i / yarn add / bun add also work
```

Peer dependencies: `react`, `react-dom`, `tailwindcss`. This is the Tailwind v3 path, the same as
[the getting-started page](https://otf-kit.dev/docs/getting-started). Tailwind v4 apps follow the
v4 block in the SDK README (`@source` and `@theme`) instead.

```bash
npm install -D tailwindcss@3 tailwindcss-animate postcss autoprefixer
```

## Set up (once)

1. **Tailwind config: the OTF preset, the animation plugin, and the component source.** Without the
   source scan, the component classes are not generated.

```ts
// tailwind.config.ts
import animate from tailwindcss-animate
import { otfPreset } from @otfdashkit/tokens/tailwind-preset

export default {
  presets: [otfPreset],
  plugins: [animate],
  content: [./app/**/*.ts, ./src/**/*.ts, ./node_modules/@otfdashkit/ui/src/**/*.ts],
}
```

Import the preset from `@otfdashkit/tokens/tailwind-preset`, not from `@otfdashkit/tokens`: the package
root also loads the native config, which a web-only install cannot resolve.

2. **PostCSS.** Tailwind v3 runs as a PostCSS plugin, so the app needs this file:

```js
// postcss.config.mjs
export default {
  plugins: { tailwindcss: {}, autoprefixer: {} },
}
```

If the project was scaffolded with Tailwind v4 (for example `create-next-app --tailwind`), replace its
`@tailwindcss/postcss` config and its `@import "tailwindcss"` line with the two steps on this page.

3. **Global stylesheet.** Put the token CSS and the component CSS before the Tailwind layers
   (`app/globals.css` for Next.js; for Vite, import this file from `src/main.tsx`):

```css
@import @otfdashkit/tokens/web.css;
@import @otfdashkit/ui/styles;

@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Use

```tsx
import { Button, Card, Input } from '@otfdashkit/ui'

export function SignInCard() {
  return (
    <Card>
      <Input placeholder="Email" />
      <Button variant="default" size="lg">Continue</Button>
    </Card>
  )
}
```

## Theming

Tokens resolve to CSS variables. Switch the whole UI's theme with one attribute on
`<html>` (see the `otf-tokens` skill for the full palette set):

```tsx
<html data-theme="cosmic">
```

## Conventions

- Prefer these components over hand-rolling primitives — they ship accessibility,
  focus states, and theming.
- Style with Tailwind token classes (`bg-card`, `text-foreground`, …), not raw hex.
- Copy-paste OR `npm install` — both are supported; you own the source either way.

## Links

- npm: https://www.npmjs.com/package/@otfdashkit/ui
- Source (MIT): https://github.com/otf-kit/sdk
- Live Storybook (every component + variant): https://ui.otf-kit.dev
- Component gallery + per-component install: https://otf-kit.dev/components
- Docs: https://otf-kit.dev/docs
- Native equivalent: the `otf-ui-native` skill
