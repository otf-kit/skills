---
name: otf-ui-native
description: >-
  Build native iOS / Android (and web) UI with @otfdashkit/ui-native — a free,
  open-source (MIT) Expo / React Native component library that mirrors
  @otfdashkit/ui's component names and props. Use when building an Expo app and you
  want the same <Button>, <Card>, <Input> API as the web SDK, wrapped once with
  <OTFProvider>. One codebase, iOS + Android + web.
license: MIT
metadata:
  source: otf-kit.dev
  package: "@otfdashkit/ui-native"
  npm: https://www.npmjs.com/package/@otfdashkit/ui-native
  repo: github.com/otf-kit/sdk
---

# @otfdashkit/ui-native — native components

A free, MIT Expo / React Native component library built on Tamagui-core. It mirrors
`@otfdashkit/ui` (web) **component names and props one-for-one**, so a screen you
know on web reads the same on native — one codebase ships iOS, Android, and web.

## When to use this skill

Activate when building or extending an **Expo / React Native** app and you want the
opinionated, accessible OTF component API on native — the same `<Button>`, `<Card>`,
`<Input>`, etc. as `@otfdashkit/ui`, with theming handled for you.

## Install

```bash
pnpm add @otfdashkit/ui-native @otfdashkit/tokens
```

Peer dependencies (Expo / Tamagui stack): `tamagui`, `@tamagui/core`,
`@tamagui/config`, `@tamagui/lucide-icons`, `expo-router`, `react`, `react-native`,
`react-native-reanimated`, `react-native-svg`. Install any your app doesn't already have.

## Set up (once)

Wrap your app root in `<OTFProvider>` — it hides the `createTamagui` + config call so
you never write Tamagui boilerplate:

```tsx
// app/_layout.tsx (Expo Router)
import { OTFProvider } from '@otfdashkit/ui-native'

export default function RootLayout() {
  return (
    <OTFProvider>
      {/* your navigator / screens */}
    </OTFProvider>
  )
}
```

## Use

```tsx
import { Button, Card, Input } from '@otfdashkit/ui-native'
// Same names and props as @otfdashkit/ui on the web.
```

## Conventions

- Wrap the root **once** in `<OTFProvider>` — no `createTamagui` config needed.
- Theme through `@otfdashkit/tokens` (see the `otf-tokens` skill) so web and native
  stay visually identical.
- Reach for these components before raw React Native primitives — they ship the
  shared API, theming, and accessibility.

## Links

- npm: https://www.npmjs.com/package/@otfdashkit/ui-native
- Source (MIT): https://github.com/otf-kit/sdk
- Live native showcase (phone-framed, scan to run): https://native-preview.otf-kit.dev
- Docs: https://otf-kit.dev/docs
- Web equivalent: the `otf-ui` skill
