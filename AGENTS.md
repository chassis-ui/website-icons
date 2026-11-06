# AGENTS.md

Instructions for AI coding agents working in this repository. It holds the rules an agent
breaks without being told; the README explains the rest.

## Project

@chassis-ui/website-icons holds the icons of the Chassis website: one SVG file per icon
in `source/`, built with [`@chassis-ui/icons`](https://www.npmjs.com/package/@chassis-ui/icons)
into `dist/`: `dist/svgs/`, one optimized file per icon, `dist/web/`, the icon font
`chassis-icons` with its stylesheets and the SVG sprite, `dist/icons.json`, the index of the
set, and for apps `dist/ios/`, the asset catalog `ChassisIcons.xcassets` and the enum `CxIcon`,
and `dist/android/`, the vector drawables `cx_<name>`. `dist/` is not committed: the release
builds it, publishes the web part to npm, and attaches an archive per target.
`chassis.build` in `package.json` names the font, the class prefix `cx-`, the frame, the
styles `outline`, `solid` and `brand`, of which `outline` and `solid` come in pairs, and the
targets `ios` and `android`. The build is file-driven: a file saved into `source/` is
an icon of the next build, and there is no list to maintain. The page of the set, which
`pnpm preview` writes into `preview/`, is in the web archive of each release.

## Commands

Once, with Node.js 22.12 or later: `pnpm install`.

| Command              | What it does                                                                 |
| -------------------- | ---------------------------------------------------------------------------- |
| `pnpm build`         | Builds `dist/`, `codepoints.json`, `README.md` and the manifest              |
| `pnpm lint`          | Checks the files of `source/`: names, frame, color, pairs                    |
| `pnpm verify`        | Fails when the build fails, or a committed file is not what it writes        |
| `pnpm preview`       | Writes the page of the set into `preview/` and serves it                     |
| `pnpm archive`       | Writes `archives/chassis-icons-<target>-<version>.zip`: web, ios and android |
| `pnpm release-notes` | Prints the changelog entry of the version, or fails                          |

`node_modules/.bin/chassis-icons --help` prints every command and option: `build --only <step>`,
`build --dry-run` and `verify --since <ref>` among them.

## Rules

- **Edit `source/`, never what the build writes.** `pnpm build` writes `dist/`, which is not
  committed: the release builds it again. It also writes `codepoints.json`, `README.md` and
  the fields `main`, `style`, `sass`, `files`, `exports` and `sideEffects` of `package.json`:
  commit those with the change to `source/`. `preview/` and `archives/` are written from
  `dist/` and are not committed.
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

## Branches

- `main` holds the released versions, and takes a change by pull request only.
- `next` is the prerelease channel. Work is merged into it first, and it is merged into `main`
  with a merge commit, never a squash, so the two keep one history.
- Any other branch is short-lived, `feat/…`, `fix/…` or `chore/…`, and is squash-merged.

## Releases

A push to `main` or `next` with a new version in `package.json` releases it, so:

- On `next`, only a version with a prerelease part, such as `0.4.0-next.0`, is released, as a
  prerelease; on `main`, any new version. Each version, a prerelease too, has its entry.
- A new version and its entry in `CHANGELOG.md` go in one commit. The release stops without
  the entry. The entry names the icons that are added, renamed or removed.
- Before that commit, run `pnpm verify`. The release builds `dist/` with the new version in
  the headers of the stylesheets.
- Do not push, tag or raise the version unless asked.
