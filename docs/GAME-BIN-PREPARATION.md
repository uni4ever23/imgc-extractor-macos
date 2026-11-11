# `game.bin` Preparation

The extractor treats disk-image decoding and executable preparation as separate layers. IMGCv2 is a block container; supported `game.bin` files inside the `game_dynamic` filesystem may use an additional product-specific wrapper.

## Discovery

The dynamic ext filesystem is selected by the `game_dynamic` label. If that label is absent, exactly one unlabelled ext partition must contain `game.bin`, `db_primary.tar.gz`, `golf/` and `lib/`. Ambiguous candidates are rejected.

## Supported executable forms

- Existing ELF32 little-endian i386 executable: copied without decryption
- Product-14 wrapped container, revision 2 or 3, at most 128 MiB: decoded and verified
- Other formats, products and revisions: extracted unchanged and reported as unsupported

## Decode and integrity process

For supported wrappers, a 256-bit payload key is derived from authenticated container fields. The implementation uses SHA-1 and RC4 to decode the wrapped key material, reverses the format's per-chunk byte permutation, and decrypts the executable body with AES-256 ECB without padding.

The result is accepted only when all of the following succeed:

- Decoded marker and ELF32 i386 structure checks
- Verification-header comparison
- Internal SHA-1 payload check
- Embedded DSA-style signature consistency check
- SHA-256 identity check before and after writing the output

The embedded signature establishes internal container consistency; it does not independently authenticate a publisher or guarantee runtime compatibility.

## Output preservation

Preparation occurs only in the extractor's private staging directory. The exact wrapped input becomes `game.bin.encrypted`; the decoded, disk-verified executable becomes `game.bin`. Existing backups with a different identity cause extraction to stop rather than overwrite evidence.

No original loader binary, disk image, game payload, or secret external key is included in this project.
