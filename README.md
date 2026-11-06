# Atomic Skills for Claude Code

Claude Code plugins for [Atomic CSS](https://atomiccss.dev) and markup conventions. Each plugin ships its skills together with its MCP server, so installing the plugin is all you need.

| Plugin | Skills | MCP server |
|---|---|---|
| `atomic-css` | `atomic` (class authoring), `figma-to-atomic` (Figma → Atomic) | `atomic-css` (`mcp.atomiccss.dev/sse`) |
| `markup-convention` | `markup` (structure, naming, accessibility) | `convention-lint` (`mcp.atomiccss.dev/convention/sse`) |

## Install

```
/plugin marketplace add Yoodaekyung/atomic-skills
/plugin install atomic-css@atomic-skills
/plugin install markup-convention@atomic-skills
```

`figma-to-atomic` reads designs through the official Figma plugin (`figma@claude-plugins-official`).

## Usage

```
/atomic 3-column card grid that collapses to 1 column on mobile
/figma-to-atomic https://www.figma.com/design/...
/markup review src/components/Header.tsx
```

## Reference

`plugins/atomic-css/skills/atomic/reference.md` is generated from the Atomic CSS build (`npm run sync:skills` in the atomic repo). Do not edit it by hand.

## Migration

This repository replaces `atomic-css-skill`, `figma-to-atomic-css-skill`, and `markup-skill`.

## Links

- [Docs](https://atomiccss.dev)
- [VS Code Extension](https://marketplace.visualstudio.com/items?itemName=Drangon-Knight.atomicss)
- [npm CLI](https://www.npmjs.com/package/atomic-css-cli)

## License

MIT
