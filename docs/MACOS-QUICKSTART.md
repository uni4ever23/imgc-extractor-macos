# macOS Quick Start

## Requirements

- Apple Silicon Mac (`arm64`)
- A completed `.imgcv2`, legacy `.imgc`, or decompressed `.img` source
- A destination with enough free space for the extraction plus safety margin

The release is self-contained. It does not require Homebrew, Python, Linux, administrator privileges, or a separate .NET installation.

## Install

Download `imgc-extractor-1.1.0-macos-arm64.tar.gz`, then run:

```sh
tar -xzf imgc-extractor-1.1.0-macos-arm64.tar.gz
cd imgc-extractor-macos-arm64
./imgc-extractor --version
```

## Inspect first

```sh
./imgc-extractor inspect "/Volumes/Source/game.imgcv2"
```

Inspection opens the source read-only, indexes the container, validates GPT and supported filesystems, inventories the selected content, and verifies whether `game.bin` can be prepared. It does not extract files.

## Extract

```sh
./imgc-extractor extract "/Volumes/Source/game.imgcv2" "/Volumes/Destination/game-extracted"
```

The destination is a positional argument. It must be a new path that does not already exist; its parent may exist. This prevents accidental merging or overwriting.

Each copied file is reread and verified with SHA-256. A supported wrapped executable produces:

```text
game.bin.encrypted   exact original container
game.bin             decoded and verified ELF32 i386 executable
```

The finished directory includes `Extraction-report.json` and `Extraction.log`. A failed or cancelled run remains in a uniquely named `.partial-*` directory for diagnosis.

## Troubleshooting

`extract requires SOURCE and a NEW OUTPUT folder` means the command needs exactly two positional paths. Do not use `--output`.

`Choose a NEW output folder` means the final destination already exists. Choose a different name; the program will create it.

If a pasted multiline command behaves unexpectedly, paste the entire command on one line. Rich-text sources can insert non-breaking spaces that the shell treats as part of an argument.
