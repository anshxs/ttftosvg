# TTF / OTF to SVG Path Generator

A small, browser based tool that turns text into SVG vector outlines using a
font file you provide. The generated SVG contains `<path>` elements, so it does
not depend on the font being installed on the machine that opens the file.

## Features

- Load TrueType (`.ttf`) and OpenType (`.otf`) font files.
- Convert each line of entered text into vector paths.
- Adjust font size, padding, line spacing, alignment, and fill colour.
- Choose a transparent background or add a solid background colour.
- Preview the result in the page and download it as an SVG file.
- Work locally in the browser; the selected font is read with the File API and
  is not uploaded by this page.

## Getting started

There is no build step or package installation. The project is a static HTML
page.

1. Open `index.html` in a modern browser, or serve the repository with any
   static file server.
2. Upload a `.ttf` or `.otf` font.
3. Enter the text and adjust the output settings.
4. Select **Download Clean SVG**.

For example, with Python available:

```sh
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

The page loads [opentype.js 1.3.4](https://www.npmjs.com/package/opentype.js)
from jsDelivr, so an internet connection is required for the font parsing
library unless that script is hosted locally instead.

## Controls

| Control | Purpose |
| --- | --- |
| Font file | Select a `.ttf` or `.otf` font from your device. |
| Text | Text to convert. New lines are preserved as separate lines. |
| Font size | Output size in SVG user units; defaults to `120`. |
| Padding | Space around the text; defaults to `30`. |
| Line spacing | Multiplier applied to the font size between baselines; defaults to `1.2`. |
| Alignment | Align each line left, centre, or right within the widest line. |
| Text colour | Fill colour for the generated paths. |
| Transparent background | Omit the background rectangle when checked. |
| Background colour | Solid rectangle colour when transparency is off. |

The exported SVG dimensions are calculated from the widest line, line spacing,
font size, and padding. The downloaded filename is based on the first line of
text, with unsafe filename characters replaced.

## Project structure

```text
ttftosvg/
├── index.html   # Complete application: markup, styles, and JavaScript
└── README.md    # Project documentation
```

There is no separate source, build output, dependency manifest, or test suite.
The application is intentionally contained in `index.html`:

- **HTML** defines the font upload, text and output controls, preview, and
  download action.
- **CSS** provides the dark responsive layout and preview styling.
- **JavaScript** loads the font with opentype.js, creates SVG path data,
  updates the preview, and triggers the SVG download.

## Implementation notes and limits

- Font parsing and SVG generation happen in the browser. Only `.ttf` and
  `.otf` extensions are accepted by the file picker and validation.
- Each text line is converted to a path using the selected font. The output
  uses the entered fill colour and, optionally, a solid background rectangle.
- Line width is measured from advance widths with kerning enabled. This is a
  simple text layout tool; it does not provide per-character positioning,
  rich text, font discovery, or advanced shaping for every writing system.
- The SVG references no font after conversion, but it depends on the browser's
  support for the SVG features used by the font outlines.
- The opentype.js script is currently loaded from a third-party CDN. The page
  itself does not send the chosen font to a server.

## Development

Edit `index.html` directly. Since there is no build configuration, changes can
be checked by opening the page in a browser and trying a font file, multiline
text, the layout controls, and the download action.

## License

No license file is currently included. Add a `LICENSE` file before distributing
or reusing the project if you want to grant others explicit reuse rights.
