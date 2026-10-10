---
name: otf-tokens
description: >-
  Theme any app with @otfdashkit/tokens — the free, open-source (MIT) shared
  design-token system behind the OTF SDK. CSS variables on web, native tokens on
  mobile, so web and native share one set of design tokens. Ships ready-made palettes
  (Slate, Warm, Cosmic, Terminal, plus Mono, Lavender, Glacier, Forest, Obsidian,
  Solar, Orchid, Indigo) you switch with one attribute. Use to theme an app, build
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

Import the stylesheet once at your app root, then set the palette with a class or
`data-theme` attribute on `<html>`:

```tsx
import '@otfdashkit/tokens/web.css'
```

```html
<html data-theme="cosmic">      <!-- or class="theme-cosmic" -->
<html data-theme="cosmic" class="dark">   <!-- dark variant -->
```

Feature code reads the tokens as CSS variables / Tailwind token classes
(`bg-background`, `text-foreground`, `border-border`, …) — never raw hex.

## Ready-made palettes

`Slate` · `Warm` · `Cosmic` · `Terminal` · `Mono` · `Lavender` · `Glacier` ·
`Forest` · `Obsidian` · `Solar` · `Orchid` · `Indigo` — each with a light and dark
mode. Add your own by defining a `.theme-<name>` block following the same shape.

## Native

On Tamagui / Expo, the tokens are consumed through `@otfdashkit/ui-native`'s
`<OTFProvider>` (see the `otf-ui-native` skill) — the same palette names apply, so a
theme switch re-tints the whole app.

## Links

- npm: https://www.npmjs.com/package/@otfdashkit/tokens
- Source (MIT): https://github.com/otf-kit/sdk
- Docs: https://otf-kit.dev/docs
- Used by: the `otf-ui` (web) and `otf-ui-native` (native) skills
