# IMGC Extractor for macOS

A self-contained Apple Silicon Terminal tool for extracting supported Golden Tee disk-image layouts directly from completed HDD Raw Copy Tool `.imgcv2`, legacy `.imgc`, or raw `.img` files.

## Download

Download the latest archive from the [Releases page](https://github.com/uni4ever23/imgc-extractor-macos/releases/latest). No Homebrew, Python, Linux, administrator privileges, or separate .NET installation is required.

## Quick start

```sh
tar -xzf imgc-extractor-1.1.0-macos-arm64.tar.gz
cd imgc-extractor-macos-arm64
./imgc-extractor inspect "/path/to/image.imgcv2"
./imgc-extractor extract "/path/to/image.imgcv2" "/path/to/new-output-folder"
```

The output path is positional and must not already exist. The source is always opened read-only.

## What it does

- Reads completed IMGCv2 2.0-2.2 containers directly without creating a full intermediate image.
- Supports raw, zero-fill, FF-fill and LZ4 records, plus legacy IMGC and raw IMG inputs.
- Discovers and reads GPT/ext partitions, then merges the supported `game_static`, `game_dynamic`, and `db_backup` layout.
- Rereads every copied file and verifies it with SHA-256.
- Checks supported product-14 `game.bin` wrappers and preserves the original as `game.bin.encrypted` before publishing the decoded ELF as `game.bin`.
- Refuses existing output folders and leaves failed/cancelled work in a uniquely named partial folder with diagnostics.

## Documentation

- [macOS Quick Start](docs/MACOS-QUICKSTART.md)
- [IMGCv2 Support](docs/IMGCV2-SUPPORT.md)
- [`game.bin` Preparation](docs/GAME-BIN-PREPARATION.md)

## Supported platform

The release targets Apple Silicon (`osx-arm64`). Intel Macs are not included in version 1.1.0.

## Integrity

Release SHA-256 checksums are provided as separate assets. The executable is ad-hoc code signed and the package is self-contained.

## License and acknowledgements

This macOS work is derived from the MIT-licensed IMGC Extractor 1.0.2 extraction core. Legacy IMGC behavior is based on `shiz/unimgc` under the WTFPL. Read-only ext support uses DiscUtils under MIT. IMGCv2 LZ4 decoding uses K4os.Compression.LZ4 under MIT.

HDD Raw Copy Tool v2.6 was inspected only for file-format interoperability. No HDD Raw Copy Tool executable, disk image, game payload, original loader binary, or private sample is distributed here.
