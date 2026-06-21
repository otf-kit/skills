# OTF Agent Skills

Free, MIT [Agent Skills](https://www.skills.sh/otf-kit/skills) for building **cross-platform UIs** with
the open-source [OTF SDK](https://github.com/otf-kit/sdk) — the same component API on
web, iOS, and Android. Works with **Claude Code, Cursor, Codex, Windsurf, Gemini CLI,
Vercel Eve, and 70+ skills-compatible agents**.

[![skills.sh](https://skills.sh/b/otf-kit/skills)](https://skills.sh/otf-kit/skills)

## Install

```bash
# all skills
npx skills add otf-kit/skills

# or pick one
npx skills add otf-kit/skills --skill otf-ui
```

Skills install into your agent's skills directory (e.g. `.claude/skills/`) and load on
demand — your agent reads only each skill's name + description until a task matches.

## What's inside

| Skill | Use it to… |
|---|---|
| [`otf-cross-platform-ui`](skills/otf-cross-platform-ui/SKILL.md) | Build one UI for web + native; decide which OTF package to import |
| [`otf-ui`](skills/otf-ui/SKILL.md) | Build accessible **web** UI with `@otfdashkit/ui` (React · Radix · Tailwind v4) |
| [`otf-ui-native`](skills/otf-ui-native/SKILL.md) | Build **native** iOS/Android/web UI with `@otfdashkit/ui-native` (Expo) |
| [`otf-tokens`](skills/otf-tokens/SKILL.md) | Theme any app with `@otfdashkit/tokens` — same palette on web + native |

Every skill is built on the **free, MIT** packages published on npm
(`@otfdashkit/ui`, `@otfdashkit/ui-native`, `@otfdashkit/tokens`). No lock-in — you own
the source.

## See it live

- Web components (Storybook): https://ui.otf-kit.dev
- Native showcase (scan to run on your phone): https://native-preview.otf-kit.dev
- Component gallery: https://otf-kit.dev/components
- SDK source: https://github.com/otf-kit/sdk

## Building a whole app?

The SDK is free and stands on its own. If you'd rather not wire the backend from
scratch, OTF also ships full-stack **kits** (SaaS dashboard, fitness, booking) — the
same components dropped into a complete, owned starting point with auth, database,
payments, and deploy scripts. Details at [otf-kit.dev](https://otf-kit.dev).

## License

[MIT](LICENSE) — free forever.
