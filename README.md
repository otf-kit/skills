# OTF Agent Skills

**Installable agent skills for auditing an app before launch and building with the OTF SDK.**

[Skill catalog](https://www.skills.sh/otf-kit/skills) · [SDK source](https://github.com/otf-kit/sdk) · [Web demo](https://ui.otf-kit.dev/) · [Native demo](https://native-preview.otf-kit.dev/)

[![Screenshot of components built with the OTF SDK](https://cdn.otf-kit.dev/readme/components-motion.gif)](https://otf-kit.dev/components)

## Install

```bash
npx skills add otf-kit/skills
```

To install one skill, pass its name:

```bash
npx skills add otf-kit/skills --skill ship-ready
```

## Skills

| Skill | Purpose |
| --- | --- |
| [`ship-ready`](skills/ship-ready/SKILL.md) | Review an app and write a prioritized launch-readiness plan. |
| [`otf-cross-platform-ui`](skills/otf-cross-platform-ui/SKILL.md) | Plan UI shared across web and Expo. |
| [`otf-ui`](skills/otf-ui/SKILL.md) | Use `@otfdashkit/ui` in a React web app. |
| [`otf-ui-native`](skills/otf-ui-native/SKILL.md) | Use `@otfdashkit/ui-native` in an Expo app. |
| [`otf-tokens`](skills/otf-tokens/SKILL.md) | Apply `@otfdashkit/tokens` themes. |

The SDK packages are available on [npm](https://www.npmjs.com/org/otfdashkit). Browse their output in the [web Storybook](https://ui.otf-kit.dev/) and [native showcase](https://native-preview.otf-kit.dev/).

## Contributing

Read [CONTRIBUTING.md](CONTRIBUTING.md) before proposing a skill change or reporting a problem.

## License

[MIT](LICENSE)
