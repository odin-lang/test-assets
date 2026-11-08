# 7z test assets

7z archives made with 7-Zip and with libarchive's 7z writer (`bsdtar --format 7zip`) from the source files below. Encrypted archives use the password `ODIN`. All pass `7z t`.

## Source files

| File | Size | CRC-32 | Contents |
|---|---:|---|---|
| ARM32.BIN | 131,072 | 2819a7f6 | 128 KiB of 32-bit ARM code: the `.text` of Odin's `examples/demo`, built for `linux_arm32` and linked with lld. |
| ARM64.BIN | 131,072 | 793f446d | 128 KiB of ARM64 code from the `__text` section of a macOS build of the Odin compiler. |
| BINARY.BIN | 100,000 | dab9f4c0 | The first 100,000 bytes of a Windows build of the Odin compiler. |
| EMPTY.TXT | 0 | 00000000 | Empty file. |
| LICENSE | 892 | b047bfa7 | Odin's zlib license text. |
| ONE.TXT | 1 | d3d99e8b | The single byte `A`. |
| PCM16.BIN | 65,536 | e13dbf6a | 16-bit stereo PCM: two sine waves per channel plus a little noise. |
| PROG.EXE | 132,096 | 1b3cc05f | X86.BIN behind the PE headers of the same Odin build, so 7-Zip treats it as an executable. |
| RANDOM.BIN | 100,000 | d4029d57 | Random bytes: incompressible. |
| RGBA.BIN | 65,536 | 70411c37 | A 128 x 128 RGBA gradient with a checkered alpha channel. |
| RISCV.BIN | 131,072 | d7fc9bf8 | 128 KiB of RISC-V code: the `.text` of Odin's `examples/demo`, built for `linux_riscv64` and linked with lld. |
| SAME.BIN | 32,768 | 6733dede | All `A`: long runs and maximum match lengths. |
| SMALL.TXT | 250 | 917123cb | Pseudo-random English-like text built from the words of LICENSE. |
| TEXT.TXT | 80,000 | 61c54dab | Pseudo-random English-like text, like SMALL.TXT. |
| THUMB.BIN | 131,072 | 98184d55 | 128 KiB of Thumb-2 code: as ARM32.BIN, built with `-target-features:thumb-mode`. |
| TONE.WAV | 65,580 | 2a5059b6 | PCM16.BIN with a RIFF WAVE header, so 7-Zip treats it as audio. |
| X86.BIN | 131,072 | ccb58444 | 128 KiB of x86-64 code from the `.text` section of a Windows build of the Odin compiler. |

`tree.7z` and `empty-only.7z` hold a `TREE` directory: `README.TXT` (SMALL.TXT), `SUB/DEEP/TEXT.TXT` (TEXT.TXT), an empty `SUB/EMPTY.TXT`, an empty `EMPTYDIR`, a read-only `READONLY.TXT`, and `ÜBER.TXT` and `日本語.TXT` with short UTF-8 text.
The libarchive archives hold the `tree` directory described in [TAR/README.md](../TAR/README.md); on Windows without `link.txt` and `日本語.TXT`.

## 7-Zip

Made with 7-Zip 26.02 (x64). Sources have their times set to 2026-11-08 12:00:00 UTC.

| File | Members | Command | Method (`7z l -slt`) | Blocks | Solid | What it exercises |
|---|---|---|---|---|---|---|
| `aes-copy.7z` | TEXT.TXT | `7z a -pODIN -m0=Copy` | Copy 7zAES | 1 | - | AES-256 over Copy (password `ODIN`). |
| `aes-headers.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -pODIN -mhe=on` | LZMA2:192k 7zAES | 1 | + | AES-256 encrypted data and headers (password `ODIN`). |
| `aes.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -pODIN` | LZMA2:192k 7zAES | 1 | + | AES-256 encrypted data (password `ODIN`), names in the clear. |
| `arm-code.7z` | ARM32.BIN | `7z a -mf=ARM` | LZMA2:17 ARM | 1 | - | ARM + LZMA2, on 32-bit ARM code. |
| `arm.7z` | BINARY.BIN | `7z a -mf=ARM` | LZMA2:17 ARM | 1 | - | ARM + LZMA2, on x86 code. |
| `arm64.7z` | ARM64.BIN | `7z a -mf=ARM64` | ARM64 LZMA2:17 | 1 | - | ARM64 + LZMA2. |
| `armt-code.7z` | THUMB.BIN | `7z a -mf=ARMT` | LZMA2:17 ARMT | 1 | - | ARM Thumb + LZMA2, on Thumb-2 code. |
| `armt.7z` | BINARY.BIN | `7z a -mf=ARMT` | LZMA2:17 ARMT | 1 | - | ARM Thumb + LZMA2, on x86 code. |
| `auto.7z` | TEXT.TXT, PROG.EXE, TONE.WAV | `7z a ` | Delta LZMA2:384k BCJ | 3 | - | Defaults: 7-Zip reads the PE and WAV headers and picks BCJ and Delta:4 itself, so the files end up in different folders with different coder chains. |
| `bcj-deflate.7z` | X86.BIN | `7z a -m0=BCJ -m1=Deflate` | Deflate BCJ | 1 | - | BCJ ahead of Deflate instead of an LZMA coder. |
| `bcj-delta.7z` | X86.BIN | `7z a -m0=BCJ -m1=Delta:2 -m2=LZMA2` | Delta LZMA2:17 BCJ | 1 | - | Three coders in one folder: BCJ, Delta, then LZMA2. |
| `bcj.7z` | X86.BIN | `7z a -mf=BCJ` | LZMA2:17 BCJ | 1 | - | BCJ x86 + LZMA2. |
| `bcj2.7z` | X86.BIN | `7z a -mf=BCJ2` | LZMA2:17 LZMA:17 BCJ2 | 1 | - | BCJ2: four input streams into one coder, with LZMA for each stream. |
| `bzip2-mt.7z` | TEXT.TXT, RANDOM.BIN | `7z a -m0=BZip2:d=100k -mmt=4` | BZip2 | 1 | + | BZip2 with 100 KB blocks, multi-threaded. |
| `bzip2.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=BZip2` | BZip2 | 1 | + | BZip2. |
| `copy.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=Copy` | Copy | 5 | - | Copy (stored). |
| `deflate.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=Deflate` | Deflate | 1 | + | Deflate. |
| `deflate64.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=Deflate64` | Deflate64 | 1 | + | Deflate64. |
| `delta-4.7z` | PCM16.BIN | `7z a -mf=Delta:4` | Delta LZMA2:16 | 1 | - | Delta, distance 4, + LZMA2. |
| `empty-only.7z` | EMPTY.TXT, TREE/EMPTYDIR | `7z a ` |  | 0 | - | Only an empty file and an empty directory: no packed streams at all. |
| `empty.7z` | none | `7z a, then 7z d of the only file` |  | 0 | - | An archive with no entries. |
| `header-notimes.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -mtm=off` | LZMA2:192k | 1 | + | No timestamps at all. |
| `header-plain.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -mhc=off` | LZMA2:192k | 1 | + | Uncompressed header (`kHeader` instead of `kEncodedHeader`). |
| `header-times.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -mtc=on -mta=on -mtm=on` | LZMA2:192k | 1 | + | Creation, access and modification times. |
| `ia64.7z` | BINARY.BIN | `7z a -mf=IA64` | LZMA2:17 IA64 | 1 | - | IA-64 + LZMA2, on x86 code. |
| `lzma-lc0-lp2-pb0.7z` | ARM64.BIN | `7z a -m0=LZMA:lc=0:lp=2:pb=0` | LZMA:17 | 1 | - | LZMA with lc=0, lp=2, pb=0 in the coder properties. |
| `lzma-lc4.7z` | TEXT.TXT | `7z a -m0=LZMA:lc=4:lp=0:pb=0` | LZMA:96k | 1 | - | LZMA with lc=4. |
| `lzma.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=LZMA` | LZMA:192k | 1 | + | LZMA. |
| `lzma2-chunks.7z` | TEXT.TXT, RANDOM.BIN, SAME.BIN | `7z a -m0=LZMA2:c=32k -mmt=4` | LZMA2:15 | 1 | + | LZMA2 with 32 KiB chunks (`c=32k`) for four threads, which also caps the dictionary at 32 KiB: dictionary and state resets inside one stream, plus uncompressed chunks. |
| `lzma2.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=LZMA2` | LZMA2:192k | 1 | + | LZMA2, the default method. |
| `ppc.7z` | BINARY.BIN | `7z a -mf=PPC` | LZMA2:17 PPC | 1 | - | PowerPC + LZMA2, on x86 code. |
| `ppmd-o2.7z` | TEXT.TXT | `7z a -m0=PPMd:o=2:mem=1m` | PPMD | 1 | - | PPMd with order 2 and a 1 MiB model: model restarts. |
| `ppmd-o32.7z` | TEXT.TXT | `7z a -m0=PPMd:o=32:mem=16m` | PPMD | 1 | - | PPMd with order 32 and a 16 MiB model. |
| `ppmd.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -m0=PPMd` | PPMD | 1 | + | PPMd (variant H, 7z flavour). |
| `riscv-code.7z` | RISCV.BIN | `7z a -mf=RISCV` | RISCV LZMA2:17 | 1 | - | RISC-V + LZMA2, on RISC-V code. |
| `riscv.7z` | BINARY.BIN | `7z a -mf=RISCV` | RISCV LZMA2:17 | 1 | - | RISC-V + LZMA2, on x86 code. |
| `solid-2f.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -ms=2f` | LZMA2:192k | 3 | + | Solid blocks of at most two files. |
| `solid-e.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -ms=e` | LZMA2:192k | 3 | + | A new solid block for each file extension. |
| `solid-off.7z` | TEXT.TXT, BINARY.BIN, SMALL.TXT, LICENSE, ONE.TXT, EMPTY.TXT | `7z a -ms=off` | LZMA2:17 | 5 | - | Non-solid: one folder per file. |
| `sparc.7z` | BINARY.BIN | `7z a -mf=SPARC` | LZMA2:17 SPARC | 1 | - | SPARC + LZMA2, on x86 code. |
| `swap2.7z` | PCM16.BIN | `7z a -m0=Swap2 -m1=LZMA2` | LZMA2:16 Swap2 | 1 | - | Swap2 (byte swap of 16-bit words) + LZMA2. |
| `swap4.7z` | RGBA.BIN | `7z a -m0=Swap4 -m1=LZMA2` | LZMA2:16 Swap4 | 1 | - | Swap4 (byte swap of 32-bit words) + LZMA2. |
| `tree.7z` | TREE | `7z a -mtc=on` | LZMA2:96k | 1 | + | Directories, an empty directory, an empty file, a read-only file and UTF-16 names outside Latin-1. |
| `volumes.7z.001 .. volumes.7z.003` | TEXT.TXT, RANDOM.BIN, BINARY.BIN | `7z a -v64k` | LZMA2:384k | 1 | + | A multi-volume set of 64 KiB volumes. |
| `x9.7z` | TEXT.TXT, PROG.EXE, TONE.WAV | `7z a -mx=9` | Delta LZMA2:384k LZMA:384k BCJ2 | 3 | - | Level 9 defaults: BCJ2 with LZMA2 and two `lc0:lp2` LZMA coders for the PE, Delta:4 for the WAV, three folders. |

## libarchive

| File | Made with | Command | What it exercises |
|---|---|---|---|
| `libarchive-mac-bzip2.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=bzip2` | libarchive's 7z writer on macOS: bzip2. |
| `libarchive-mac-copy.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=copy` | libarchive's 7z writer on macOS: copy. |
| `libarchive-mac-deflate.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=deflate` | libarchive's 7z writer on macOS: deflate. |
| `libarchive-mac-lzma1.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=lzma1` | libarchive's 7z writer on macOS: lzma1. |
| `libarchive-mac-lzma2.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=lzma2` | libarchive's 7z writer on macOS: lzma2. |
| `libarchive-mac-ppmd.7z` | macOS 27.0.1 bsdtar 3.5.3 | `tar --format 7zip --options 7zip:compression=ppmd` | libarchive's 7z writer on macOS: ppmd. |
| `libarchive-win-copy.7z` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format 7zip --options 7zip:compression=copy -cf libarchive-win-copy.7z tree` | libarchive's 7z writer on Windows: Copy. |
| `libarchive-win-deflate.7z` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format 7zip --options 7zip:compression=deflate -cf libarchive-win-deflate.7z tree` | libarchive's 7z writer on Windows: Deflate. |
