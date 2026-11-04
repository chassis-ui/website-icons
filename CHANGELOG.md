# Changelog

The notable changes of each release of `@chassis-ui/website-icons`, in the form of
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Written by hand, in the commit that
sets the version. The body of the GitHub release is the entry of its version, and a version
without an entry is not released. An entry names the icons that are added, renamed or removed.

The history of the set before 0.1.0 is in the
[changelog of Chassis Icons](https://github.com/chassis-ui/icons/blob/main/packages/icons/CHANGELOG.md),
up to `@chassis-ui/icons` 0.4.0-next.0.

## [0.1.0]

### Added

- The icons of the Chassis website, moved from `@chassis-ui/icons`, with the 501 icons
  and the code points of its 0.4.0-next.0: the font `chassis-icons` with the class prefix
  `cx-`, in the styles `outline`, `solid` and `brand`. A project that installed
  `@chassis-ui/icons` for the set installs `@chassis-ui/website-icons`, and no icon changes
  its name or its code point. The files are in `dist/`: `icons/chassis-icons.*` is
  `dist/web/chassis-icons.*`, and `svgs/<name>.svg` is `dist/svgs/<name>.svg`.
- `dist/icons.json`, the index of the set: the font name, the class prefix, the frame, the
  styles and the pairs, and each icon with its code point, its style and its name without
  the style.
- The page of the set, which `pnpm preview` writes into `preview/` and serves, and which the
  archive of a release holds.
- The icons for apps, in the archives of each release beside
  `chassis-icons-web-<version>.zip`. `chassis-icons-ios-<version>.zip`: the asset catalog
  `ChassisIcons.xcassets`, a template vector image per icon, and `ChassisIcons.swift`, the enum
  `CxIcon` with `Image(CxIcon.<name>)` and `UIImage(CxIcon.<name>)`.
  `chassis-icons-android-<version>.zip`: a vector drawable per icon, `R.drawable.cx_<name>`.
  The npm package holds the files for the web only.
- Built with `@chassis-ui/icons` 0.4.0, which is the builder of icon sets from this version on.
