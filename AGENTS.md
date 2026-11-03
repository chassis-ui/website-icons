# AGENTS.md

Instructions for AI coding agents working in this repository. It holds the rules an agent
breaks without being told; the README explains the rest.

## Project

@chassis-ui/website-icons holds the icons of the Chassis Design System: one SVG file per icon
in `source/`, built with [`@chassis-ui/icons`](https://www.npmjs.com/package/@chassis-ui/icons)
into `dist/`: `dist/svgs/`, one optimized file per icon, `dist/web/`, the icon font
`chassis-icons` with its stylesheets and the SVG sprite, and `dist/icons.json`, the index of
the set. `chassis.build` in `package.json` names the font, the
class prefix `cx-`, the frame and the styles `outline`, `solid` and `brand`, of which
`outline` and `solid` come in pairs. The build is file-driven: a file saved into `source/` is
an icon of the next build, and there is no list to maintain. The set is shown on
[chassis-ui.com/icons](https://chassis-ui.com/icons/), by the site of
[chassis-ui/icons](https://github.com/chassis-ui/icons), which vendors this repository.

## Commands

Once, with Node.js 22.12 or later: `pnpm install`.

| Command              | What it does                                                                    |
| -------------------- | ------------------------------------------------------------------------------- |
| `pnpm build`         | Builds `dist/`, `codepoints.json`, `README.md` and the manifest                 |
| `pnpm lint`          | Checks the files of `source/`: names, frame, color, pairs                       |
| `pnpm verify`        | Fails when the output differs from a fresh build                                |
| `pnpm preview`       | Writes the page of the set into `preview/` and serves it                        |
| `pnpm archive`       | Writes `archives/chassis-icons-web-<version>.zip`, with the output and the page |
| `pnpm release-notes` | Prints the changelog entry of the version, or fails                             |

`node_modules/.bin/chassis-icons --help` prints every command and option: `build --only <step>`,
`build --dry-run` and `verify --since <ref>` among them.

## Rules

- **Edit `source/`, never the output.** `dist/`, `codepoints.json`, `README.md` and the
  fields `main`, `style`, `sass`, `files`, `exports` and `sideEffects` of `package.json` are
  written by `pnpm build`. Commit them with the change to `source/`. `preview/` and
  `archives/` are written from the output and are not committed.
- **A file of `source/` is an icon.** Its name is the name of the icon, in kebab-case, and
  ends in a style of `chassis.build` when the set has styles. `pnpm lint` says what else a
  file needs: the frame, one color, no stroke.
- **An icon keeps its code point.** `codepoints.json` is the registry of the code points: a new
  icon gets the next free one, and the code point of a removed icon is retired. The build is its
  only writer.
- **The names of the icons are public.** A project reads an icon by name: the class
  `cx-<name>`, the sprite symbol `<name>` and the file `dist/svgs/<name>.svg`. Renaming or
  removing an icon breaks it, so that is the maintainer's decision, and the changelog says so.
  `chassis.checks.json` lists the icons that a project reads by name, and `pnpm verify`
  fails when one is missing.
- **Do not empty `chassis.checks.json` to get past `pnpm verify`.** The layouts of
  `@chassis-ui/docs` and the default icons of Chassis React read those icons by name: a
  missing one is a broken Chassis site or component. Restore the icon, or ask.
- **An icon of a style has its pair.** An `outline` icon comes with its `solid` icon and the
  other way round; `pnpm lint` says so when one is missing.

## Releases

A push to `main` with a new version in `package.json` releases it, so:

- A new version and its entry in `CHANGELOG.md` go in one commit. The release stops without
  the entry. The entry names the icons that are added, renamed or removed.
- Before that commit, run `pnpm build`, which writes the version into the stylesheets, and
  `pnpm verify`.
- Do not push, tag or raise the version unless asked.
