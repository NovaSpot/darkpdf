# DarkPDF

Turn any PDF from light mode to dark mode, right in your browser.

- **Text stays selectable and searchable.** The output is still a real vector PDF, not screenshots.
- **Images keep their colours.** Photos are not turned into negatives.
- **Private by design.** Everything runs client-side. Your file never leaves your device.
- **Easy on the eyes.** Soft black and soft white instead of harsh pure black and white, and a calm, muted interface with no bright colours, even for errors.

> **Status:** early prototype (v1). See [Known limitations](#known-limitations).

## Features

- Upload a PDF by choosing a file or dragging it onto the page
- Live dark preview with selectable text
- Download the converted PDF
- Three background shades: Soft black, Charcoal, Warm dark
- Scanned-document mode for PDFs that are just page images
- Progress bar while converting, and lazy page loading so large files open quickly

## Quick start

No build step and no dependencies to install.

```bash
git clone <your-repo-url>
cd <your-repo-folder>
npx serve .
```

Then open the address it prints. You can also open `index.html` directly in a browser, but serving it is more reliable.

An internet connection is needed because `pdf-lib` and `pdf.js` load from cdnjs.

## How it works

Dimmer does not rasterize pages. For every page it:

1. Paints an opaque white page behind the original content, so blending always has a backdrop.
2. Adds three full-page layers on top using PDF blend modes:
   - **Difference** with white, which inverts everything.
   - **Lighten** with a dark colour, which turns pure black into soft black.
   - **Darken** with a light colour, which turns pure white into soft off-white.
3. Pre-flips each image with its `/Decode` array, a lossless operation with no re-encoding, so the page-wide inversion turns it back into its original colours.

Because the original text and fonts are never touched, the text stays selectable and the file size barely changes. The preview is rendered from the converted file, so what you see is what you download.

## Tech

| Part | Library |
| --- | --- |
| PDF editing and export | [pdf-lib](https://pdf-lib.js.org/) 1.17.1 |
| Preview and text layer | [PDF.js](https://mozilla.github.io/pdf.js/) 3.11.174 |
| UI | Plain HTML, CSS, and JavaScript in a single file |

## Known limitations

- CMYK, palette (Indexed), and JPEG 2000 images are not corrected and will look like negatives. Inline images are skipped.
- Photos get very slightly lifted blacks and capped whites from the soft-black and soft-white layers.
- Scanned PDFs need the "Scanned document" option, otherwise the page stays white.
- Coloured vector content such as charts and links turns into complementary colours.
- Password-protected PDFs are not supported.
- Blend-mode support depends on the PDF viewer. Acrobat, Chrome, Firefox, PDF.js, and Preview handle it, but very old or minimal viewers may not.

## Roadmap

- [ ] Handle CMYK, Indexed, and inline images
- [ ] Saturation slider to mute loud colours
- [ ] Direct colour remapping in the content stream
- [ ] OCR text layer for scanned documents
- [ ] Batch conversion and remembered settings
- [ ] Test suite with sample PDFs (text paper, slide export, photo brochure, scan, chart report)

## Contributing

Issues and pull requests are welcome. If a PDF converts badly, please open an issue and describe what looks wrong. If you can share the file, that helps a lot.


