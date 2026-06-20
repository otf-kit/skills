---
name: otf-cross-platform-ui
description: >-
  Build UIs that render the same on web, iOS, and Android from one component API,
  using OTF's free open-source (MIT) SDK. Use this when starting a cross-platform
  UI, deciding which OTF package to import, or when you want the same <Button>,
  <Card>, <Input> — same names, same props, same look — on web and native. Routes
  to the web (@otfdashkit/ui), native (@otfdashkit/ui-native), and theming
  (@otfdashkit/tokens) packages.
license: MIT
metadata:
  source: otf-kit.dev
  repo: github.com/otf-kit/sdk
  packages: "@otfdashkit/ui, @otfdashkit/ui-native, @otfdashkit/tokens"
---

# OTF — one component API, web + native

OTF is a free, MIT-licensed cross-platform UI SDK. The core idea: `<Button>` from
**`@otfdashkit/ui`** (web — React, on Radix + Tailwind) and `<Button>` from
**`@otfdashkit/ui-native`** (native — Expo / React Native) share the **same name,
the same props, and the same look**. Build once, ship to the browser and to the
App Store. Own the source — copy-paste or `npm install`, no lock-in.

## When to use this skill

Activate when building a UI that targets web and/or native and you want a single,
accessible, opinionated component API. Use it to decide which package to import and
to wire the SDK up correctly the first time.

## Which package do I import?

| You're building for… | Import | Renders with |
|---|---|---|
| **Web** (Next.js, Vite, React) | `@otfdashkit/ui` | Radix + Tailwind v4 |
| **Native** (Expo, React Native — iOS/Android/web) | `@otfdashkit/ui-native` | Tamagui-core |
| **Theming for either/both** | `@otfdashkit/tokens` | CSS vars on web, native tokens on mobile |

The component **names and props match across `@otfdashkit/ui` and `@otfdashkit/ui-native`** —
learn the API once, use it everywhere.

```tsx
// Web
import { Button, Card, Input } from '@otfdashkit/ui'

// Native — same names, same props. That's the whole point.
import { Button, Card, Input } from '@otfdashkit/ui-native'
```

## Go deeper

- Web components → the `otf-ui` skill
- Native components → the `otf-ui-native` skill
- Theming / design tokens → the `otf-tokens` skill

## Links

- Source (free, MIT): https://github.com/otf-kit/sdk
- Live web components (Storybook): https://ui.otf-kit.dev
- Live native showcase: https://native-preview.otf-kit.dev
- Component gallery + per-component install: https://otf-kit.dev/components
- Docs: https://otf-kit.dev/docs
