---
name: otf-cross-platform-ui
description: >-
  Build UIs for web, iOS, and Android from one component set, using OTF's free
  open-source (MIT) SDK. Use this when starting a cross-platform UI, deciding which
  OTF package to import, or when you want <Button>, <Card>, <Input> under the same
  names and the same design tokens on web and native. Props differ per platform. Routes
  to the web (@otfdashkit/ui), native (@otfdashkit/ui-native), and theming
  (@otfdashkit/tokens) packages.
license: MIT
metadata:
  source: otf-kit.dev
  repo: github.com/otf-kit/sdk
  packages: "@otfdashkit/ui, @otfdashkit/ui-native, @otfdashkit/tokens"
---

# OTF — one component set, web + native

OTF is a free, MIT-licensed cross-platform UI SDK. The core idea: `<Button>` from
**`@otfdashkit/ui`** (web — React, on Radix + Tailwind) and `<Button>` from
**`@otfdashkit/ui-native`** (native — Expo / React Native) share the **same name**
and the **same design tokens**. They do **not** share the same props: each package has
its own variants and sizes. Own the source — copy-paste or `npm install`, no lock-in.

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

The component **names match for the components both packages ship, and both packages read
the same `@otfdashkit/tokens`**. Props do not match: check each component's page or source
before you copy props from one platform to the other. For example, web `Button` takes
`variant` of `default | secondary | outline | ghost | destructive | link | gradient | texture | shimmer`
and `size` of `sm | default | lg | xl | icon | icon-sm`; native `Button` takes `variant` of
`primary | default | outlined | transparent | floating | destructive | shimmer` and a Tamagui
size token such as `"$4"`.

```tsx
// Web
import { Button, Card, Input } from '@otfdashkit/ui'

// Native — same component names, its own props (see above).
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
