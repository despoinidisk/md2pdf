# md2pdf

Convert a Markdown file to PDF. The output is written next to the input with the
same name and a `.pdf` extension, unless another path is given.

## Usage

```sh
./md2pdf path/to/report.md
./md2pdf path/to/report.md -o path/to/custom.pdf
./md2pdf path/to/report.md --export-diagrams
```

`--export-diagrams` also saves each rendered diagram as both PNG and SVG in
`path/to/report-diagrams/` (next to the PDF).

Diagrams are scaled down if needed to fit the page height. For diagrams that
support it (flowcharts, state, class and ER diagrams), `--diagram-direction LR`
lays them out left-to-right for a shorter, wider footprint (less downscaling in
the PDF):

```sh
./md2pdf path/to/report.md --diagram-direction LR
```

Diagram types without a direction setting (for example sequence diagrams) are
left as authored by Mermaid.

`--square-layout` renders each diagram in both TB and LR and keeps whichever
aspect ratio is closer to 1:1. This changes the actual Mermaid layout, but it is
best-effort: Mermaid's layout engine decides the shape, so a long chain still
cannot become truly square without rewriting the diagram source.

`--pad-square` is a purely cosmetic alternative: it pads the shorter axis with
transparent space so the canvas is square. It does not rearrange the diagram.

```sh
./md2pdf path/to/report.md --square-layout
./md2pdf path/to/report.md --pad-square --export-diagrams
```

## Install

One-line install (downloads the CLI and installs Python/npm dependencies):

```sh
curl -fsSL https://github.com/despoinidisk/md2pdf/raw/main/install | bash
```

By default the script is installed to `/usr/local/bin` when writable, otherwise
to `~/.local/bin`. Override with `MD2PDF_BIN_DIR`. To skip dependency
installation, set `MD2PDF_SKIP_DEPS=1`. If the repository location differs, set
`MD2PDF_BASE_URL` to the directory containing `md2pdf` and `requirements.txt`.

Manual install:

```sh
python3 -m pip install -r requirements.txt
chmod +x md2pdf
```

Mermaid diagrams (fenced with ` ```mermaid `) are rendered to PNG and embedded in
the PDF, so `mmdc` from mermaid-cli must be available on `PATH`. The installer
tries to install it automatically; otherwise:

```sh
npm install -g @mermaid-js/mermaid-cli
```

If a document contains Mermaid blocks but `mmdc` is missing, conversion stops
with an error instead of silently producing a PDF with raw diagram code.
