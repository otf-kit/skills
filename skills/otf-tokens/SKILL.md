---
name: otf-tokens
description: >-
  Theme any app with @otfdashkit/tokens — the free, open-source (MIT) shared
  design-token system behind the OTF SDK. CSS variables on web, native tokens on
  mobile, so web and native share one set of design tokens. Ships ready-made palettes
  (Slate, Warm, Cosmic, Terminal) you switch with one class. Use to theme an app, build
  a theme switcher, or keep colors consistent across platforms.
license: MIT
metadata:
  source: otf-kit.dev
  package: "@otfdashkit/tokens"
  npm: https://www.npmjs.com/package/@otfdashkit/tokens
  repo: github.com/otf-kit/sdk
---

# @otfdashkit/tokens — shared design tokens

The free, MIT design-token layer behind the OTF SDK. The same palette resolves to
**CSS variables on web** and **native design tokens on mobile**, so a `@otfdashkit/ui`
web app and a `@otfdashkit/ui-native` app share one visual language. Works on its own
in any web or Tamagui app, too.

## When to use this skill

Activate to theme an app, add or switch a color palette, build a theme switcher, or
keep web + native on the same design tokens.

## Install

```bash
pnpm add @otfdashkit/tokens
```

## Web

Import the stylesheet once at your app root, then set the palette with a `theme-<name>` class
on `<html>`:

```tsx
import '@otfdashkit/tokens/web.css'
```

```html
<html class="theme-cosmic">
<html class="theme-cosmic dark">   <!-- dark variant -->
```

In React or Next.js (`app/layout.tsx`), write `className` instead of `class` on `<html>`.

Feature code reads the tokens as CSS variables / Tailwind token classes
(`bg-background`, `text-foreground`, `border-border`, …) — never raw hex. Those classes need the
OTF Tailwind preset (`@otfdashkit/tokens/tailwind-preset`) in `tailwind.config.js`; the otf-ui
skill's Set up step shows the config.

## Ready-made palettes

`Slate` · `Warm` · `Cosmic` · `Terminal` — the class names are lowercase (`theme-slate`, `theme-warm`,
`theme-cosmic`, `theme-terminal`). These four are defined in the web stylesheet
(`@otfdashkit/tokens/web.css`), each with a light and dark mode. Add your own by defining a
`.theme-<name>` block following the same shape.

## Native

On Tamagui / Expo, the tokens are consumed through `@otfdashkit/ui-native`'s
`<OTFProvider>` (see the `otf-ui-native` skill) — the same palette names apply, so a
theme switch re-tints the whole app.

## Links

- npm: https://www.npmjs.com/package/@otfdashkit/tokens
- Source (MIT): https://github.com/otf-kit/sdk
- Docs: https://otf-kit.dev/docs
- Used by: the `otf-ui` (web) and `otf-ui-native` (native) skills
