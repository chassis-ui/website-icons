# @chassis-ui/website-icons

The icons of the Chassis Design System, in outline, solid and brand styles, as an icon font, an SVG sprite and optimized SVG files, built with @chassis-ui/icons

The set has 501 icons. Each is drawn on a frame of 24 by 24 in one color, and takes the color of the text around it.

```bash
npm install @chassis-ui/website-icons
```

## What the package holds

- `dist/svgs/<name>.svg`: one optimized file per icon
- `dist/web/chassis-icons.css` and `dist/web/chassis-icons.min.css`: the stylesheet of the icon font, with one class per icon
- `dist/web/chassis-icons.scss`: the same stylesheet for Sass, with the folder of the font and the map of the icons as variables
- `dist/web/chassis-icons.woff2`, `dist/web/chassis-icons.woff`: the icon font, which the stylesheets load from their own folder
- `dist/web/chassis-icons.svg`: the SVG sprite, one `<symbol>` per icon with the name of the icon as its `id`
- `dist/web/chassis-icons.json`: the code point of each icon in the font, by name
- `dist/icons.json`: the index of the set: its font name, class prefix, frame and styles, and each icon with its code point and its style

The last part of a name is the style of the icon: `<name>-outline`, `<name>-solid` or `<name>-brand`. The examples below show `alarm-clock-outline`.

Browse the icons and their names on the [site of the set](https://chassis-ui.com/icons/).

## Use

### The icon font

One class per icon, `cx-<name>`, on an empty element:

```html
<link rel="stylesheet" href="node_modules/@chassis-ui/website-icons/dist/web/chassis-icons.min.css" />

<i class="cx-alarm-clock-outline"></i>
```

With a bundler, `import '@chassis-ui/website-icons'` loads the same stylesheet and its font.

### The SVG sprite

One file for the whole set, and one `<use>` per icon:

```html
<svg width="24" height="24" fill="currentcolor" aria-hidden="true">
  <use href="node_modules/@chassis-ui/website-icons/dist/web/chassis-icons.svg#alarm-clock-outline"></use>
</svg>
```

### A single SVG file

```html
<img src="node_modules/@chassis-ui/website-icons/dist/svgs/alarm-clock-outline.svg" alt="" width="24" height="24" />
```

### Sass

The classes of the stylesheet, with the font served from a folder of your own, and the map of the icons for a selector of your own:

```scss
@use 'sass:map';
@use '@chassis-ui/website-icons/dist/web/chassis-icons.scss' as icons with (
  $chassis-icons-font-dir: '/fonts'
);

.my-icon::before {
  font-family: icons.$chassis-icons-font;
  content: map.get(icons.$chassis-icons-map, 'alarm-clock-outline');
}
```

Sass finds the file in a load path that holds `node_modules`, and so does a bundler. With the package importer of Sass, `@use 'pkg:@chassis-ui/website-icons'` loads the same file.

## The source

The package is built from the SVG files of its [repository](https://github.com/chassis-ui/website-icons) with [`@chassis-ui/icons`](https://www.npmjs.com/package/@chassis-ui/icons). An icon is added or changed there, and never in the files of `dist/`. `pnpm build` writes the output again, `pnpm lint` checks the files of the source, and `pnpm verify` checks that the output is what the source builds. `pnpm preview` writes the page of the set into `preview/` and serves it, to browse the icons, their classes, their code points and the code that draws them.

## License

MIT
