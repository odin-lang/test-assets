# XZ test assets

`.xz` streams, and two legacy `.lzma` files, made with XZ Utils and 7-Zip from the source files below, plus XZ Utils' own test files. Every `.xz` file we made passes `xz -t`; the source files themselves aren't included, so the tables give their sizes and CRC-32s.

## Source files

| File | Size | CRC-32 | Contents |
|---|---:|---|---|
| ARM32.BIN | 131,072 | 2819a7f6 | 128 KiB of 32-bit ARM code: the `.text` of Odin's `examples/demo`, built for `linux_arm32` and linked with lld. |
| ARM64.BIN | 131,072 | 793f446d | 128 KiB of ARM64 code from the `__text` section of a macOS build of the Odin compiler. |
| BINARY.BIN | 100,000 | dab9f4c0 | The first 100,000 bytes of a Windows build of the Odin compiler. |
| EMPTY.TXT | 0 | 00000000 | Empty file. |
| ONE.TXT | 1 | d3d99e8b | The single byte `A`. |
| PCM16.BIN | 65,536 | e13dbf6a | 16-bit stereo PCM: two sine waves per channel plus a little noise. |
| RANDOM.BIN | 100,000 | d4029d57 | Random bytes: incompressible. |
| RGBA.BIN | 65,536 | 70411c37 | A 128 x 128 RGBA gradient with a checkered alpha channel. |
| RISCV.BIN | 131,072 | d7fc9bf8 | 128 KiB of RISC-V code: the `.text` of Odin's `examples/demo`, built for `linux_riscv64` and linked with lld. |
| SAME.BIN | 32,768 | 6733dede | All `A`: long runs and maximum match lengths. |
| SMALL.TXT | 250 | 917123cb | Pseudo-random English-like text built from the words of LICENSE. |
| TEXT.TXT | 80,000 | 61c54dab | Pseudo-random English-like text, like SMALL.TXT. |
| THUMB.BIN | 131,072 | 98184d55 | 128 KiB of Thumb-2 code: as ARM32.BIN, built with `-target-features:thumb-mode`. |
| X86.BIN | 131,072 | ccb58444 | 128 KiB of x86-64 code from the `.text` section of a Windows build of the Odin compiler. |

## XZ Utils

Made with xz (XZ Utils) 5.8.3.

| File | Source | Command | What it exercises |
|---|---|---|---|
| `arm.bcj.xz` | ARM32.BIN | `xz --arm --lzma2` | BCJ ARM on 32-bit ARM code. |
| `arm64.bcj-start.xz` | ARM64.BIN | `xz --arm64=start=4096 --lzma2` | BCJ ARM64 with a start offset. |
| `arm64.bcj.xz` | ARM64.BIN | `xz --arm64 --lzma2` | BCJ ARM64 on ARM64 code. |
| `arm64.lc0-lp2-pb0.xz` | ARM64.BIN | `xz --lzma2=preset=6,lc=0,lp=2,pb=0` | LZMA2 with lc=0, lp=2, pb=0. |
| `armthumb.bcj.xz` | THUMB.BIN | `xz --armthumb --lzma2` | BCJ ARM-Thumb on Thumb-2 code. |
| `binary.bcj-arm.xz` | BINARY.BIN | `xz --arm --lzma2` | BCJ arm, on x86 code: still has to round-trip. |
| `binary.bcj-armthumb.xz` | BINARY.BIN | `xz --armthumb --lzma2` | BCJ armthumb, on x86 code: still has to round-trip. |
| `binary.bcj-ia64.xz` | BINARY.BIN | `xz --ia64 --lzma2` | BCJ ia64, on x86 code: still has to round-trip. |
| `binary.bcj-powerpc.xz` | BINARY.BIN | `xz --powerpc --lzma2` | BCJ powerpc, on x86 code: still has to round-trip. |
| `binary.bcj-riscv.xz` | BINARY.BIN | `xz --riscv --lzma2` | BCJ riscv, on x86 code: still has to round-trip. |
| `binary.bcj-sparc.xz` | BINARY.BIN | `xz --sparc --lzma2` | BCJ sparc, on x86 code: still has to round-trip. |
| `concat-padding.xz` | TEXT.TXT, SMALL.TXT | `cat with zero padding` | Stream padding between and after streams, plus an empty stream in the middle. |
| `concat.xz` | TEXT.TXT, SMALL.TXT | `cat of two `xz` streams` | Two concatenated streams with different checks; decodes to TEXT.TXT followed by SMALL.TXT. |
| `empty.lzma` | EMPTY.TXT | `xz --format=lzma` | An empty `.lzma` file. |
| `empty.xz` | EMPTY.TXT | `xz ` | Empty input: a stream with no blocks and an empty index. |
| `one.xz` | ONE.TXT | `xz ` | One byte. |
| `pcm16.delta-2.xz` | PCM16.BIN | `xz --delta=dist=2 --lzma2` | Delta, distance 2. |
| `pcm16.delta-4.xz` | PCM16.BIN | `xz --delta=dist=4 --lzma2` | Delta, distance 4 (16-bit stereo frames). |
| `random.xz` | RANDOM.BIN | `xz ` | Incompressible input: LZMA2 uncompressed chunks. |
| `rgba.delta-256.xz` | RGBA.BIN | `xz --delta=dist=256 --lzma2` | Delta, the maximum distance 256. |
| `rgba.delta-4.xz` | RGBA.BIN | `xz --delta=dist=4 --lzma2` | Delta, distance 4 (RGBA pixels). |
| `riscv.bcj-start.xz` | RISCV.BIN | `xz --riscv=start=4096 --lzma2` | BCJ RISC-V with a start offset. |
| `riscv.bcj.xz` | RISCV.BIN | `xz --riscv --lzma2` | BCJ RISC-V on RISC-V code. |
| `same.xz` | SAME.BIN | `xz ` | One repeated byte: maximum match lengths. |
| `text.block-list.xz` | TEXT.TXT | `xz --block-list=1KiB,10KiB,0` | Uneven blocks from --block-list: 1 KiB, 10 KiB, then the rest. |
| `text.blocks-16k.xz` | TEXT.TXT | `xz --block-size=16KiB` | Multiple blocks: five full 16 KiB blocks, sizes recorded in the index only. |
| `text.check-crc32.xz` | TEXT.TXT | `xz -6 --check=crc32` | Integrity check CRC32. |
| `text.check-crc64.xz` | TEXT.TXT | `xz -6 --check=crc64` | Integrity check CRC64. |
| `text.check-none.xz` | TEXT.TXT | `xz -6 --check=none` | Integrity check None. |
| `text.check-sha256.xz` | TEXT.TXT | `xz -6 --check=sha256` | Integrity check SHA256. |
| `text.delta-1.xz` | TEXT.TXT | `xz --delta=dist=1 --lzma2` | Delta, distance 1. |
| `text.dict-1536m.xz` | TEXT.TXT | ``xz`, then the dictionary byte patched to 37 and the block header CRC32 recomputed` | A 1.5 GiB dictionary in the filter properties, for an 80,000-byte file: allocate by the data, not the header. |
| `text.dict-4k.xz` | TEXT.TXT | `xz --lzma2=preset=6,dict=4KiB` | LZMA2 with the minimum dictionary, 4 KiB. |
| `text.dict-max.xz` | TEXT.TXT | ``xz`, then the dictionary byte patched to 40 and the block header CRC32 recomputed` | The largest dictionary LZMA2 can declare, 4 GiB - 1, for an 80,000-byte file. |
| `text.fast-hc4.xz` | TEXT.TXT | `xz --lzma2=preset=6,mode=fast,mf=hc4` | LZMA2 fast mode with the hc4 match finder. |
| `text.lc4-lp0-pb0.xz` | TEXT.TXT | `xz --lzma2=preset=6,lc=4,lp=0,pb=0` | LZMA2 with lc=4, the maximum with lp=0. |
| `text.lzma` | TEXT.TXT | `xz --format=lzma` | The legacy `.lzma` format (lzma-alone), for comparison. |
| `text.pb4.xz` | TEXT.TXT | `xz --lzma2=preset=6,pb=4` | LZMA2 with pb=4, the maximum. |
| `text.preset-0.xz` | TEXT.TXT | `xz -0` | Preset 0. |
| `text.preset-1.xz` | TEXT.TXT | `xz -1` | Preset 1. |
| `text.preset-9.xz` | TEXT.TXT | `xz -9` | Preset 9. |
| `text.preset-9e.xz` | TEXT.TXT | `xz -9e` | Preset 9e. |
| `text.threads.xz` | TEXT.TXT | `xz -T4 --block-size=16KiB` | Multi-threaded: compressed and uncompressed sizes in every block header. |
| `x86.bcj-start.xz` | X86.BIN | `xz --x86=start=4096 --lzma2` | BCJ x86 with a start offset. |
| `x86.bcj.xz` | X86.BIN | `xz --x86 --lzma2` | BCJ x86 on x86-64 code. |
| `x86.block-filters.xz` | X86.BIN | `xz --filters1=x86 lzma2 --filters2=delta:dist=4 lzma2 --block-list=1:32KiB,2:32KiB,0:32KiB,1:0` | A different filter chain per block: BCJ x86, Delta, the default, then BCJ x86 again. |
| `x86.chain-4.xz` | X86.BIN | `xz --delta=dist=2 --x86 --arm64 --lzma2` | The longest chain: three filters before LZMA2. |

## 7-Zip

Made with 7-Zip 26.02 (x64).

| File | Source | Command | What it exercises |
|---|---|---|---|
| `7z.arm.bcj.xz` | ARM32.BIN | `7z a -txz -mf=ARM` | BCJ ARM on 32-bit ARM code. |
| `7z.arm64.bcj.xz` | ARM64.BIN | `7z a -txz -mf=ARM64` | BCJ ARM64. |
| `7z.armthumb.bcj.xz` | THUMB.BIN | `7z a -txz -mf=ARMT` | BCJ ARM-Thumb on Thumb-2 code. |
| `7z.binary.riscv.xz` | BINARY.BIN | `7z a -txz -mf=RISCV` | BCJ RISC-V, on x86 code. |
| `7z.empty.xz` | EMPTY.TXT | `7z a -txz ` | Empty input. Unlike `xz`, 7-Zip writes one empty block. |
| `7z.pcm16.delta-4.xz` | PCM16.BIN | `7z a -txz -mf=Delta:4` | Delta, distance 4. |
| `7z.random.xz` | RANDOM.BIN | `7z a -txz ` | Incompressible input. |
| `7z.riscv.bcj.xz` | RISCV.BIN | `7z a -txz -mf=RISCV` | BCJ RISC-V on RISC-V code. |
| `7z.text.blocks.xz` | TEXT.TXT | `7z a -txz -mmt=2 -ms=16k` | Multi-threaded with 16 KiB blocks: five blocks. |
| `7z.text.mx9.xz` | TEXT.TXT | `7z a -txz -mx=9` | Level 9. |
| `7z.text.xz` | TEXT.TXT | `7z a -txz ` | Defaults. |
| `7z.x86.bcj.xz` | X86.BIN | `7z a -txz -mf=BCJ` | BCJ x86. |

## XZ Utils test files

`xz-utils/` holds the `.xz` and `.lzma` files from `tests/files` of the XZ Utils 5.8.3 release tarball (SHA-256 `fff1ffcf2b0da84d308a14de513a1aa23d4e9aa3464d17e64b9714bfdd0bbfb6`), unchanged, with the README that says what each one tests: 25 `good-*` files must decode, 47 `bad-*` ones must fail, and 5 `unsupported-*` ones use features a decoder may reject. The lzip (`.lz`) files are left out.

`xz -t` from XZ Utils 5.8.3 passes every good file and fails every bad and unsupported one, except `unsupported-check.xz`, where it only warns about the reserved check ID (exit status 2).

They're available under the BSD Zero Clause License ([COPYING.0BSD](xz-utils/COPYING.0BSD)), Copyright (C) The XZ Utils authors and contributors.
