# ZIP test assets

Each member is one compressed stream for one ZIP compression method, copied verbatim from an archive made by the tool listed under *Sources*. Identical streams from several tool versions appear once, listing every version that produced them. Member names are `<original>.<method>.<packed size>`, with a letter appended when two different streams would otherwise share a name. CRC-32 and sizes are those recorded by the original tool.

- `pkzip-legacy.zip`: Stored (0), Shrunk (1), Reduced with compression factors 1-4 (2-5) and Imploded (6).
- `deflate.zip`: Deflated (8) and Deflate64 (9).
- `deflate64_vector_7zip.bin`: a raw Deflate64 stream written by 7-Zip, for `tests/core/compress/test_core_compress_deflate64.odin`.

## Method suffixes

- `imploded-4k2t`, `-8k3t`: a 4 or 8 KiB sliding window (general purpose flag bit 1), and two or three Shannon-Fano trees, i.e. without or with a literal tree (bit 2). PKZIP chooses both itself; none of the tools produced `4k3t` or `8k2t`.
- `deflated-n`, `-x`, `-f`, `-s`: normal, maximum, fast and super fast, from general purpose flag bits 1-2.

## Source files

All except LICENSE and EMBLEM.ICO were generated with a fixed random seed.

| File | Size | Contents |
|---|---:|---|
| BINARY.BIN | 100,000 | The first 100,000 bytes of a Windows build of the Odin compiler. |
| DLE.BIN | 16,384 | Dense in 0x90, Reduce's DLE escape byte: runs of DLE, DLE followed by 0x00, and DLE mixed with literals and text. |
| EMBLEM.ICO | 3,638 | Odin's emblem icon. |
| EMPTY.TXT | 0 | Empty file. |
| LICENSE | 892 | Odin's zlib license text. |
| ONE.TXT | 1 | The single byte `A`. |
| RANDOM.BIN | 100,000 | Random bytes. Incompressible, so every tool stores it. |
| SAME.BIN | 32,768 | All `A`: long runs, maximum match lengths, and Shrink's KwKwK case. |
| SMALL.TXT | 250 | Pseudo-random English-like text built from the words of LICENSE. |
| TEXT.TXT | 80,000 | The same kind of text. Large enough to fill Shrink's 8192-entry table, so it exercises code size increases and partial clears. |

## Tools

- PKZIP 0.80, 0.90, 0.92, 1.01, 1.02, 1.10 and 2.04g for DOS. Shrink is the default of 0.80-0.92 and `-es` on 1.10, Reduce is `-ea<n> -eb<n>`, Implode is `-ex` on 1.01-1.10, and Deflate is 2.04g at each of its four levels.
- 7-Zip 26.02 for Windows: `7z a -tzip -mm=Deflate64 -mx=1` and `-mx=9`.

## pkzip-legacy.zip

| Member | Method | Flags | Packed | Unpacked | CRC-32 | Sources |
|---|---|---|---:|---:|---|---|
| LICENSE.stored.892 | 0 | 0x0000 | 892 | 892 | b047bfa7 | PKZIP 2.04 |
| LICENSE.shrunk.603 | 1 | 0x0000 | 603 | 892 | b047bfa7 | PKZIP 0.80, PKZIP 0.90, PKZIP 0.92, PKZIP 1.10 |
| LICENSE.imploded-4k2t.589 | 6 | 0x0000 | 589 | 892 | b047bfa7 | PKZIP 1.01, PKZIP 1.02 |
| LICENSE.imploded-4k2t.590 | 6 | 0x0000 | 590 | 892 | b047bfa7 | PKZIP 1.10 |
| EMBLEM.ICO.stored.3638 | 0 | 0x0000 | 3,638 | 3,638 | 8724158d | PKZIP 2.04 |
| EMBLEM.ICO.shrunk.2772 | 1 | 0x0000 | 2,772 | 3,638 | 8724158d | PKZIP 0.90, PKZIP 1.10 |
| EMBLEM.ICO.reduced-1.2787 | 2 | 0x0000 | 2,787 | 3,638 | 8724158d | PKZIP 0.92 |
| EMBLEM.ICO.reduced-2.2775 | 3 | 0x0000 | 2,775 | 3,638 | 8724158d | PKZIP 0.92 |
| EMBLEM.ICO.reduced-3.2758 | 4 | 0x0000 | 2,758 | 3,638 | 8724158d | PKZIP 0.92 |
| EMBLEM.ICO.reduced-4.2741 | 5 | 0x0000 | 2,741 | 3,638 | 8724158d | PKZIP 0.92 |
| EMBLEM.ICO.imploded-4k2t.2558 | 6 | 0x0000 | 2,558 | 3,638 | 8724158d | PKZIP 1.10 |
| SMALL.TXT.shrunk.210 | 1 | 0x0000 | 210 | 250 | 917123cb | PKZIP 0.80, PKZIP 0.92, PKZIP 1.01, PKZIP 1.02, PKZIP 1.10 |
| TEXT.TXT.shrunk.22799 | 1 | 0x0000 | 22,799 | 80,000 | 61c54dab | PKZIP 0.80, PKZIP 0.92, PKZIP 1.10 |
| TEXT.TXT.reduced-1.36432 | 2 | 0x0000 | 36,432 | 80,000 | 61c54dab | PKZIP 0.92 |
| TEXT.TXT.reduced-2.31810 | 3 | 0x0000 | 31,810 | 80,000 | 61c54dab | PKZIP 0.92 |
| TEXT.TXT.reduced-3.28256 | 4 | 0x0000 | 28,256 | 80,000 | 61c54dab | PKZIP 0.92 |
| TEXT.TXT.reduced-4.26620 | 5 | 0x0000 | 26,620 | 80,000 | 61c54dab | PKZIP 0.80, PKZIP 0.92 |
| TEXT.TXT.imploded-8k3t.23391 | 6 | 0x0006 | 23,391 | 80,000 | 61c54dab | PKZIP 1.10 |
| TEXT.TXT.imploded-8k3t.23448 | 6 | 0x0006 | 23,448 | 80,000 | 61c54dab | PKZIP 1.01, PKZIP 1.02 |
| BINARY.BIN.reduced-1.21880 | 2 | 0x0000 | 21,880 | 100,000 | dab9f4c0 | PKZIP 0.92 |
| BINARY.BIN.reduced-2.21431 | 3 | 0x0000 | 21,431 | 100,000 | dab9f4c0 | PKZIP 0.92 |
| BINARY.BIN.reduced-3.21384 | 4 | 0x0000 | 21,384 | 100,000 | dab9f4c0 | PKZIP 0.92 |
| BINARY.BIN.reduced-4.21516 | 5 | 0x0000 | 21,516 | 100,000 | dab9f4c0 | PKZIP 0.80, PKZIP 0.92 |
| BINARY.BIN.imploded-4k2t.28836 | 6 | 0x0000 | 28,836 | 100,000 | dab9f4c0 | PKZIP 1.10 |
| BINARY.BIN.imploded-4k2t.28837 | 6 | 0x0000 | 28,837 | 100,000 | dab9f4c0 | PKZIP 1.01, PKZIP 1.02 |
| SAME.BIN.shrunk.288 | 1 | 0x0000 | 288 | 32,768 | 6733dede | PKZIP 0.80, PKZIP 0.92, PKZIP 1.10 |
| SAME.BIN.reduced-1.307 | 2 | 0x0000 | 307 | 32,768 | 6733dede | PKZIP 0.92 |
| SAME.BIN.reduced-2.327 | 3 | 0x0000 | 327 | 32,768 | 6733dede | PKZIP 0.92 |
| SAME.BIN.reduced-3.351 | 4 | 0x0000 | 351 | 32,768 | 6733dede | PKZIP 0.92 |
| SAME.BIN.reduced-4.385 | 5 | 0x0000 | 385 | 32,768 | 6733dede | PKZIP 0.80, PKZIP 0.92 |
| SAME.BIN.imploded-8k3t.505 | 6 | 0x0006 | 505 | 32,768 | 6733dede | PKZIP 1.01, PKZIP 1.02, PKZIP 1.10 |
| RANDOM.BIN.stored.100000 | 0 | 0x0000 | 100,000 | 100,000 | d4029d57 | PKZIP 0.80, PKZIP 0.92, PKZIP 1.01, PKZIP 1.02, PKZIP 1.10, PKZIP 2.04, 7-Zip 26.02 -mx1, 7-Zip 26.02 -mx9 |
| DLE.BIN.reduced-1.3955 | 2 | 0x0000 | 3,955 | 16,384 | 096501b1 | PKZIP 0.92 |
| DLE.BIN.reduced-2.3916 | 3 | 0x0000 | 3,916 | 16,384 | 096501b1 | PKZIP 0.92 |
| DLE.BIN.reduced-3.3770 | 4 | 0x0000 | 3,770 | 16,384 | 096501b1 | PKZIP 0.92 |
| DLE.BIN.reduced-4.3612 | 5 | 0x0000 | 3,612 | 16,384 | 096501b1 | PKZIP 0.80, PKZIP 0.92 |
| DLE.BIN.imploded-4k2t.2944 | 6 | 0x0000 | 2,944 | 16,384 | 096501b1 | PKZIP 1.10 |
| DLE.BIN.imploded-4k2t.2953 | 6 | 0x0000 | 2,953 | 16,384 | 096501b1 | PKZIP 1.01, PKZIP 1.02 |
| EMPTY.TXT.stored.0 | 0 | 0x0000 | 0 | 0 | 00000000 | PKZIP 0.80, PKZIP 0.92, PKZIP 1.10, PKZIP 2.04, 7-Zip 26.02 -mx1, 7-Zip 26.02 -mx9 |
| ONE.TXT.stored.1 | 0 | 0x0000 | 1 | 1 | d3d99e8b | PKZIP 0.80, PKZIP 0.92, PKZIP 1.10, PKZIP 2.04, 7-Zip 26.02 -mx1, 7-Zip 26.02 -mx9 |

## deflate.zip

| Member | Method | Flags | Packed | Unpacked | CRC-32 | Sources |
|---|---|---|---:|---:|---|---|
| LICENSE.deflated-n.481 | 8 | 0x0000 | 481 | 892 | b047bfa7 | PKZIP 2.04 |
| LICENSE.deflated-x.481 | 8 | 0x0002 | 481 | 892 | b047bfa7 | PKZIP 2.04 |
| LICENSE.deflated-f.483 | 8 | 0x0004 | 483 | 892 | b047bfa7 | PKZIP 2.04 |
| LICENSE.deflated-s.484 | 8 | 0x0006 | 484 | 892 | b047bfa7 | PKZIP 2.04 |
| LICENSE.deflated64-n.474 | 9 | 0x0000 | 474 | 892 | b047bfa7 | 7-Zip 26.02 -mx9 |
| LICENSE.deflated64-n.485 | 9 | 0x0000 | 485 | 892 | b047bfa7 | 7-Zip 26.02 -mx1 |
| EMBLEM.ICO.deflated-n.2458 | 8 | 0x0000 | 2,458 | 3,638 | 8724158d | PKZIP 2.04 |
| EMBLEM.ICO.deflated-x.2456 | 8 | 0x0002 | 2,456 | 3,638 | 8724158d | PKZIP 2.04 |
| EMBLEM.ICO.deflated-f.2460 | 8 | 0x0004 | 2,460 | 3,638 | 8724158d | PKZIP 2.04 |
| EMBLEM.ICO.deflated-s.2527 | 8 | 0x0006 | 2,527 | 3,638 | 8724158d | PKZIP 2.04 |
| SMALL.TXT.deflated-n.214 | 8 | 0x0000 | 214 | 250 | 917123cb | PKZIP 2.04 |
| SMALL.TXT.deflated-x.214 | 8 | 0x0002 | 214 | 250 | 917123cb | PKZIP 2.04 |
| SMALL.TXT.deflated-f.214 | 8 | 0x0004 | 214 | 250 | 917123cb | PKZIP 2.04 |
| SMALL.TXT.deflated-s.219 | 8 | 0x0006 | 219 | 250 | 917123cb | PKZIP 2.04 |
| SMALL.TXT.deflated64-n.177 | 9 | 0x0000 | 177 | 250 | 917123cb | 7-Zip 26.02 -mx9 |
| SMALL.TXT.deflated64-n.178 | 9 | 0x0000 | 178 | 250 | 917123cb | 7-Zip 26.02 -mx1 |
| TEXT.TXT.deflated-n.21551 | 8 | 0x0000 | 21,551 | 80,000 | 61c54dab | PKZIP 2.04 |
| TEXT.TXT.deflated-x.21495 | 8 | 0x0002 | 21,495 | 80,000 | 61c54dab | PKZIP 2.04 |
| TEXT.TXT.deflated-f.22813 | 8 | 0x0004 | 22,813 | 80,000 | 61c54dab | PKZIP 2.04 |
| TEXT.TXT.deflated-s.24947 | 8 | 0x0006 | 24,947 | 80,000 | 61c54dab | PKZIP 2.04 |
| TEXT.TXT.deflated64-n.19410 | 9 | 0x0000 | 19,410 | 80,000 | 61c54dab | 7-Zip 26.02 -mx9 |
| TEXT.TXT.deflated64-n.22266 | 9 | 0x0000 | 22,266 | 80,000 | 61c54dab | 7-Zip 26.02 -mx1 |
| BINARY.BIN.deflated-n.21391 | 8 | 0x0000 | 21,391 | 100,000 | dab9f4c0 | PKZIP 2.04 |
| BINARY.BIN.deflated-x.21390 | 8 | 0x0002 | 21,390 | 100,000 | dab9f4c0 | PKZIP 2.04 |
| BINARY.BIN.deflated-f.22006 | 8 | 0x0004 | 22,006 | 100,000 | dab9f4c0 | PKZIP 2.04 |
| BINARY.BIN.deflated-s.22699 | 8 | 0x0006 | 22,699 | 100,000 | dab9f4c0 | PKZIP 2.04 |
| BINARY.BIN.deflated64-n.20944 | 9 | 0x0000 | 20,944 | 100,000 | dab9f4c0 | 7-Zip 26.02 -mx9 |
| BINARY.BIN.deflated64-n.22207 | 9 | 0x0000 | 22,207 | 100,000 | dab9f4c0 | 7-Zip 26.02 -mx1 |
| SAME.BIN.deflated-n.47 | 8 | 0x0000 | 47 | 32,768 | 6733dede | PKZIP 2.04 |
| SAME.BIN.deflated-x.47 | 8 | 0x0002 | 47 | 32,768 | 6733dede | PKZIP 2.04 |
| SAME.BIN.deflated-f.47 | 8 | 0x0004 | 47 | 32,768 | 6733dede | PKZIP 2.04 |
| SAME.BIN.deflated-s.158 | 8 | 0x0006 | 158 | 32,768 | 6733dede | PKZIP 2.04 |
| SAME.BIN.deflated64-n.129 | 9 | 0x0000 | 129 | 32,768 | 6733dede | 7-Zip 26.02 -mx1, 7-Zip 26.02 -mx9 |
| DLE.BIN.deflated-n.2556 | 8 | 0x0000 | 2,556 | 16,384 | 096501b1 | PKZIP 2.04 |
| DLE.BIN.deflated-x.2550 | 8 | 0x0002 | 2,550 | 16,384 | 096501b1 | PKZIP 2.04 |
| DLE.BIN.deflated-f.2602 | 8 | 0x0004 | 2,602 | 16,384 | 096501b1 | PKZIP 2.04 |
| DLE.BIN.deflated-s.2681 | 8 | 0x0006 | 2,681 | 16,384 | 096501b1 | PKZIP 2.04 |
| DLE.BIN.deflated64-n.2476 | 9 | 0x0000 | 2,476 | 16,384 | 096501b1 | 7-Zip 26.02 -mx9 |
| DLE.BIN.deflated64-n.2616 | 9 | 0x0000 | 2,616 | 16,384 | 096501b1 | 7-Zip 26.02 -mx1 |

## Archives

Whole archives for testing the ZIP container rather than the compression methods. Encrypted archives use the password `ODIN`.

| File | Made with | What it exercises |
|---|---|---|
| `archives/7z-aes128.zip` | 7-Zip 26.02, `-mem=AES128 -pODIN` | WinZip AES (method 99, extra field 0x9901), 128-bit key, over Deflate. |
| `archives/7z-aes192-stored.zip` | 7-Zip 26.02, `-mem=AES192 -mm=Copy -pODIN` | WinZip AES, 192-bit key, over Store. |
| `archives/7z-aes256.zip` | 7-Zip 26.02, `-mem=AES256 -pODIN` | WinZip AES, 256-bit key, over Deflate. |
| `archives/7z-bzip2.zip` | 7-Zip 26.02, `-mm=BZip2` | BZip2 (method 12). |
| `archives/7z-deflate64.zip` | 7-Zip 26.02, `7z a -tzip -mm=Deflate64` | Deflate64 (method 9) archive. |
| `archives/7z-lzma.zip` | 7-Zip 26.02, `-mm=LZMA` | LZMA (method 14) with the end-of-stream marker (flag bit 1). |
| `archives/7z-ppmd.zip` | 7-Zip 26.02, `-mm=PPMd` | PPMd version I, revision 1 (method 98). |
| `archives/7z-store.zip` | 7-Zip 26.02, `7z a -tzip -mm=Copy` | Store (method 0) archive. |
| `archives/7z-utf8.zip` | 7-Zip 26.02, `-mcu=on` | UTF-8 names with general purpose flag bit 11: accents, Greek, Japanese, an emoji; NTFS timestamp extra field (0x000a). |
| `archives/7z-xz-code.zip` | 7-Zip 26.02, `-mm=XZ -mf=on` | XZ on x86-64 and ARM64 code. 7-Zip ignores `-mf` for zip, so these are LZMA2 only, like every method 95 archive it writes. |
| `archives/7z-xz.zip` | 7-Zip 26.02, `-mm=XZ` | XZ (method 95): each member is a whole `.xz` stream with a CRC32 check. 7-Zip stores the members XZ doesn't shrink (one, empty and 250 bytes) with method 0. |
| `archives/7z-zipcrypto.zip` | 7-Zip 26.02, `-mem=ZipCrypto -pODIN` | ZipCrypto over Deflate as written by 7-Zip. |
| `archives/ditto-utf8.zip` | macOS `ditto -c -k --sequesterRsrc --keepParent` | NFD UTF-8 names without the UTF-8 flag, plus `__MACOSX/._*` AppleDouble entries. |
| `archives/ditto.zip` | macOS `ditto -c -k --sequesterRsrc --keepParent` | Finder-style archive: data descriptors, symlinks, extra field 0x5855, and an extended attribute stored as an AppleDouble file under `__MACOSX/`. |
| `archives/explorer-cp437.zip` | Windows 10 22H2, Send to > Compressed (zipped) folder | CP437 names without the UTF-8 flag, no extra fields, directory entries for every folder, empty and tiny files stored. Explorer refuses to add names outside the OEM code page (Greek, Japanese) instead of writing UTF-8. |
| `archives/iz-4.5GiB.zip` | Info-ZIP Zip 3.0 (macOS), `-fz` | One 4,831,838,208-byte file of zeros: sizes beyond 32 bits. |
| `archives/iz-70000-entries.zip` | Info-ZIP Zip 3.0 (macOS), `-r` | 70,001 entries (a directory and 70,000 empty files): more than 65,535 entries, so the entry count needs ZIP64 end of central directory records. |
| `archives/iz-nodirs-9.zip` | Info-ZIP Zip 3.0 (macOS 27.0.1), `zip -r -D -9` | Info-ZIP maximum Deflate, no directory entries. |
| `archives/iz-split.z01` | Info-ZIP Zip 3.0 (macOS), `-0 -s 64k` | First 64 KiB part of the split archive. |
| `archives/iz-split.zip` | Info-ZIP Zip 3.0 (macOS), `-0 -s 64k` | Last part of a split archive; the first part is `iz-split.z01`. |
| `archives/iz-store.zip` | Info-ZIP Zip 3.0 (macOS 27.0.1), `zip -r -y -0` | Info-ZIP Store, with symlinks stored as links. |
| `archives/iz-stream-zip64.zip` | Info-ZIP Zip 3.0 (macOS), `zip -fz - files | cat` | Streamed and forced ZIP64: data descriptors with 8-byte sizes and ZIP64 extra fields. The end of central directory record gives the central directory offset as `0xFFFFFFFF`, yet there is no ZIP64 end of central directory record or locator, so a reader has to find the central directory from its size instead. |
| `archives/iz-stream.zip` | Info-ZIP Zip 3.0 (macOS), `zip - files | cat` | Written to a pipe: every entry has zero CRC and sizes in its local header and a data descriptor with signature (flag bit 3). |
| `archives/iz-unix.zip` | Info-ZIP Zip 3.0 (macOS), `-r -y` | Unix permissions (755, 644, 600), stored symlinks (to a file, to a directory, and one escaping the archive: `../../../etc/passwd`), empty directory. |
| `archives/iz-utf8.zip` | Info-ZIP Zip 3.0 (macOS), `-r`, UTF-8 locale | UTF-8 names in Unicode NFD (decomposed, as macOS returns them) without the UTF-8 flag; Unix extra fields UT (0x5455) and ux (0x7875). |
| `archives/iz-zip64.zip` | Info-ZIP Zip 3.0 (macOS), `-fz` | ZIP64 extra fields (0x0001) on small files, and ZIP64 end of central directory records. |
| `archives/iz-zipcrypto.zip` | Info-ZIP Zip 3.0 (macOS 27.0.1), `zip -r -y -P ODIN` | Info-ZIP ZipCrypto over Deflate. |
| `archives/libarchive-mac-aes256.zip` | macOS 27.0.1 bsdtar 3.5.3, `tar --format zip --options zip:encryption=aes256 --passphrase ODIN` | libarchive on macOS: WinZip AES-256. |
| `archives/libarchive-mac-deflate.zip` | macOS 27.0.1 bsdtar 3.5.3, `tar --format zip` | libarchive's zip writer on macOS: Deflate with data descriptors, NFD UTF-8 names without the UTF-8 flag, symlinks, a hard link. |
| `archives/libarchive-mac-store.zip` | macOS 27.0.1 bsdtar 3.5.3, `tar --format zip --options zip:compression=store` | libarchive on macOS: Store. |
| `archives/libarchive-mac-zipcrypto.zip` | macOS 27.0.1 bsdtar 3.5.3, `tar --format zip --options zip:encryption=zipcrypt --passphrase ODIN` | libarchive on macOS: ZipCrypto. |
| `archives/libarchive-win-aes256.zip` | Windows 10 22H2 bsdtar 3.8.9, `tar --format zip --options zip:encryption=aes256 --passphrase ODIN -cf libarchive-win-aes256.zip tree` | libarchive on Windows: WinZip AES-256 (password `ODIN`). |
| `archives/libarchive-win-deflate.zip` | Windows 10 22H2 bsdtar 3.8.9, `tar --format zip -cf libarchive-win-deflate.zip tree` | libarchive's zip writer on Windows: Deflate with data descriptors, Unix host and extra fields, names in the OEM code page (CP437) without the UTF-8 flag. |
| `archives/libarchive-win-store.zip` | Windows 10 22H2 bsdtar 3.8.9, `tar --format zip --options zip:compression=store -cf libarchive-win-store.zip tree` | libarchive on Windows: Store. |
| `archives/libarchive-win-zipcrypto.zip` | Windows 10 22H2 bsdtar 3.8.9, `tar --format zip --options zip:encryption=zipcrypt --passphrase ODIN -cf libarchive-win-zipcrypto.zip tree` | libarchive on Windows: ZipCrypto (password `ODIN`). |
| `archives/pkzip110-sfx.exe` | PKZIP 1.10, ZIP2EXE | Self-extracting DOS executable with an Implode and a Shrink member; offsets count from the start of the file. |
| `archives/pkzip110-zipcrypto-implode.zip` | PKZIP 1.10, `-ex -sODIN` | ZipCrypto over Implode, with both 4k2t and 8k3t members. |
| `archives/pkzip110-zipcrypto-shrink.zip` | PKZIP 1.10, `-es -sODIN` | ZipCrypto over Shrink. |
| `archives/pkzip204-comment-fake-signatures.zip` | PKZIP 2.04g, `-z < COMMENT.TXT` | The archive comment contains a fake end of central directory signature (`PK\x05\x06`) and a fake central header signature (`PK\x01\x02`). A reader that takes the first `PK\x05\x06` found scanning backwards (e.g. Python's `zipfile`) fails. |
| `archives/pkzip204-comments.zip` | PKZIP 2.04g | Archive comment and per-file comments. |
| `archives/pkzip204-paths.zip` | PKZIP 2.04g, `-rP` | Stored paths with directory entries (DOS directory attribute), including an empty directory. |
| `archives/pkzip204-sfx.exe` | PKZIP 2.04g, ZIP2EXE | Self-extracting DOS executable: the archive follows the extractor stub. ZIP2EXE rewrites the offsets, so they count from the start of the file. |
| `archives/pkzip204-zipcrypto-deflate.zip` | PKZIP 2.04g, `-ex -sODIN` | Traditional PKWARE encryption (ZipCrypto) over Deflate, plus encrypted stored empty and one-byte files. |
| `archives/pkzip204-zipcrypto-store.zip` | PKZIP 2.04g, `-e0 -sODIN` | ZipCrypto over Store. |
| `archives/powershell51.zip` | Windows PowerShell 5.1, Microsoft.PowerShell.Archive 1.0.1.0, `Compress-Archive -Path tree` | Compress-Archive on .NET Framework: backslashes as path separators, the UTF-8 flag only on non-ASCII names, directory entries only for directories without files, no extra fields. |
| `archives/pwsh7.zip` | PowerShell 7.6, Microsoft.PowerShell.Archive 1.2.6, `Compress-Archive -Path tree` | Compress-Archive on .NET: forward slashes, otherwise as Windows PowerShell 5.1. |
| `archives/xz-bcj.zip` | Assembled in Python around `xz --x86`, `xz --arm64` and plain `xz` streams (XZ Utils 5.8.3) | XZ with BCJ filters ahead of LZMA2 in the `.xz` filter chain: x86 and ARM64, plus a plain member, with CRC64 checks. No tool we have writes these, so the container is minimal: no extra fields, version 6.3. |
| `archives/xz-sha256-multiblock.zip` | Assembled in Python around an `xz --check=sha256 --block-size=16KiB` stream (XZ Utils 5.8.3) | XZ with a SHA-256 check and five blocks in one member. |
