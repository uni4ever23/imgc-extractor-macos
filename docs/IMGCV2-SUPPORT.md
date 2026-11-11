# IMGCv2 Support

The macOS reader opens completed HDD Raw Copy Tool IMGCv2 images directly. It does not create a full intermediate `.img` file.

## Supported containers

- Public format major version 2, minor versions 0 through 2
- Completed acquisitions with structurally consistent header, records and footer
- Source sector sizes recorded by the container
- Raw, zero-fill, FF-fill and LZ4 block encodings
- Indexed random access with a bounded 256 MiB decoded-block cache

Legacy `.imgc` and decompressed `.img` inputs remain supported.

## Safety checks

Before exposing the image as a virtual disk, the reader verifies container magic and version, completion state, record boundaries and ordering, logical offsets and sizes, payload lengths, footer markers, block totals and logical disk size.

Paused, incomplete, truncated or inconsistent acquisitions are rejected. Complete the acquisition with the imaging tool before extraction. The extractor never modifies the source.

## Validation

The implementation was tested with synthetic images covering all four record encodings. Decoded output matched the source SHA-256 byte-for-byte, passed 1,000 randomized reads, and matched an independently executed vendor decoder. End-to-end GPT/ext inspection and extraction were also tested.

These checks establish supported-format decoding and integrity. They are not a claim that every disk layout or filesystem feature is supported.
