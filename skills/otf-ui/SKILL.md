---
name: otf-ui
description: >-
  Build accessible, production-ready web UI with @otfdashkit/ui — a free,
  open-source (MIT) React component library on Radix + Tailwind v4. Use when adding
  buttons, forms, dialogs, sheets, tables, charts, command palettes, toasts, and
  more to a React / Next.js / Vite app, theming them, or installing components by
  npm or copy-paste. Components match @otfdashkit/ui-native one-for-one so the same
  API works on native.
license: MIT
metadata:
  source: otf-kit.dev
  package: "@otfdashkit/ui"
  npm: https://www.npmjs.com/package/@otfdashkit/ui
  repo: github.com/otf-kit/sdk
---

# @otfdashkit/ui — web components

A free, MIT React component library built on [Radix](https://www.radix-ui.com/) +
[Tailwind CSS v4](https://tailwindcss.com/). Accessible (keyboard + screen-reader)
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

Peer dependencies: `react`, `react-dom`, `tailwindcss`.

## Set up (once)

Import the tokens stylesheet and the component styles at your app root
(`app/layout.tsx` for Next.js, `src/main.tsx` for Vite):

```tsx
import '@otfdashkit/tokens/web.css'
import '@otfdashkit/ui/styles'
```

## Use

```tsx
import { Button, Card, Input } from '@otfdashkit/ui'

export function SignInCard() {
  return (
    <Card>
      <Input placeholder="Email" />
      <Button variant="primary" size="lg">Continue</Button>
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
