# tar test assets

tar archives from GNU tar, libarchive's `bsdtar` on Windows and macOS and the macOS `pax`, plus archives crafted in Python.

- `hostile-*` must be refused, or read without writing anything outside the extraction directory: path traversal, absolute and Windows device paths, symlink and hard link escapes, absurd and negative sizes, broken checksums and numbers, truncation, malformed pax records and sparse maps.
- `crafted-*` are valid but unusual archives that readers should accept.
- Everything else is what real tools write.

## Source tree

Times are 2026-11-08 12:00:00 UTC; GNU tar records the owner as `odin:1000`, the macOS tools as the user who made them.

| Path | Contents |
|---|---|
| `ids/` | Files with mtimes in 1960 and 2300, archived with uid 3000000 and gid 3000001. |
| `long/dddd…/eeee…/file.txt` | A 135-byte path that ustar can split into prefix and name. |
| `long/link-long` | A symlink with a 148-byte target. |
| `long/nnnn….txt` | A 155-byte name in one component. |
| `mv/` | TEXT.TXT and RANDOM.BIN, for the multi-volume set. |
| `sparse/sparse.bin` | A 4 MiB sparse file with six 4 KiB data regions. |
| `tree/EMPTY.TXT`, `tree/ONE.TXT` | Empty, and the single byte `A`. |
| `tree/emptydir/` | An empty directory, mode 700. |
| `tree/hard.txt` | A hard link to `README.TXT`. |
| `tree/link.txt` | A symlink to `README.TXT`. |
| `tree/README.TXT` | SMALL.TXT (250 bytes, CRC-32 917123cb). |
| `tree/run.sh` | A two-line shell script, mode 755. |
| `tree/sub/deep/LICENSE` | Odin's license, mode 600. |
| `tree/TEXT.TXT` | TEXT.TXT (80,000 bytes, CRC-32 61c54dab). |
| `tree/ÜBER.TXT`, `tree/日本語.TXT` | Short UTF-8 text under non-ASCII names. |
| `xattr/tagged.txt` | A file with a `user.odin` extended attribute. |

## GNU tar and crafted

| File | Made with | How | What it exercises |
|---|---|---|---|
| `crafted-after-end.tar` | Python | crafted | A member after the end-of-archive blocks, which readers ignore. |
| `crafted-no-end.tar` | Python | crafted | No end-of-archive zero blocks. |
| `crafted-one-end-block.tar` | Python | crafted | One end-of-archive zero block instead of two. |
| `crafted-pax-newline.tar` | Python | crafted | A pax `path` containing a newline: records are length-delimited. |
| `crafted-signed-checksum.tar` | Python | crafted | A checksum summed over signed bytes, as old Sun and BSD tars did; most readers accept it. |
| `crafted-v7-spaces.tar` | Python | crafted | A V7 header with space-padded, space-terminated numbers and no magic. |
| `crafted-xstar.tar` | Python | crafted | A Schily xstar header: a 131-byte prefix, then atime, ctime and `tar\0` at offset 508. |
| `gnu-b1.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu -b 1 tree` | Blocking factor 1: no padding to a 10240-byte record. |
| `gnu-gnu.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu tree long` | GNU: `././@LongLink` entries for long names and link targets. |
| `gnu-gnu.tar.gz` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu -z tree` | GNU format through `tar -z`. |
| `gnu-ids.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:3000000 --group=odin:3000001 --format=gnu ids` | GNU base-256 numbers: uid and gid over 2097151, mtimes before 1970 and after 2242. |
| `gnu-incremental.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu --listed-incremental=/home/jeroen/.cache/odin-tar-corpus/snar tree` | A GNU incremental dump: `D` directory entries with dumpdir contents. |
| `gnu-label.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu --label=ODIN tree` | A GNU volume label (`V`) entry. |
| `gnu-multivolume-1.tar .. -3.tar` | tar (GNU tar) 1.35 | `tar -c -M -L 64 --format=gnu --label=ODIN-MV -f ... mv` | A GNU multi-volume set with a 64 KiB tape length, rounded up to whole 10 KiB records: labels and `M` continuation entries. |
| `gnu-oldgnu.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=oldgnu tree long` | Old GNU: `././@LongLink` entries for long names and link targets. |
| `gnu-posix-global.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix --pax-option=comment=Made for Odin tree` | POSIX pax with a global (`g`) extended header. |
| `gnu-posix-ids.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:3000000 --group=odin:3000001 --format=posix ids` | The same in pax records, including a negative mtime. |
| `gnu-posix-members.tar.gz` | Python gzip | two gzip members, split mid-block | Two concatenated gzip members: the tar continues across the member boundary. |
| `gnu-posix-xattrs.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix --xattrs xattr` | POSIX pax with a `SCHILY.xattr.user.odin` record. |
| `gnu-posix.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix tree long` | POSIX pax (GNU tar's defaults): `PaxHeaders` entries with path, linkpath, atime and ctime. |
| `gnu-posix.tar.bz2` | bzip2 1.0.8 | `bzip2 -9` of `gnu-posix.tar` | bzip2. |
| `gnu-posix.tar.gz` | gzip 1.14 | `gzip -9n` of `gnu-posix.tar` | gzip. |
| `gnu-posix.tar.lzma` | xz (XZ Utils) 5.8.3 | `xz --format=lzma` of `gnu-posix.tar` | lzma-alone. |
| `gnu-posix.tar.xz` | xz (XZ Utils) 5.8.3 | `xz -6` of `gnu-posix.tar` | xz. |
| `gnu-sparse-posix-0.0.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix --sparse --sparse-version=0.0 sparse` | POSIX sparse 0.0: `GNU.sparse.offset`/`numbytes` record pairs. |
| `gnu-sparse-posix-0.1.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix --sparse --sparse-version=0.1 sparse` | POSIX sparse 0.1: one `GNU.sparse.map` record. |
| `gnu-sparse-posix-1.0.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=posix --sparse --sparse-version=1.0 sparse` | POSIX sparse 1.0: the map at the start of the data, a `GNUSparseFile.0` name. |
| `gnu-sparse.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=gnu --sparse sparse` | GNU sparse (`S`) with six data regions: four in the header, the rest in an extension block. |
| `gnu-ustar.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=ustar tree long/dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd` | POSIX ustar: a 135-byte path split across the prefix and name fields. |
| `gnu-v7.tar` | tar (GNU tar) 1.35 | `tar -c --owner=odin:1000 --group=odin:1000 --format=v7 tree` | V7: no magic, no user or group names, directories as names ending in `/`. |
| `hostile-absolute.tar` | Python | crafted | Absolute paths: POSIX, drive letters with either slash, UNC, root-relative and `//`. |
| `hostile-case-collision.tar` | Python | crafted | Names that collide on case-insensitive file systems, including a directory and a file. |
| `hostile-checksum.tar` | Python | crafted | A header checksum off by one. |
| `hostile-devices.tar` | Python | crafted | Character and block devices, a FIFO, a contiguous file (`7`) and an unknown type (`Z`). |
| `hostile-dotdot.tar` | Python | crafted | Paths that climb out with `..`, plain and hidden behind `a/..` and `./`. |
| `hostile-duplicates.tar` | Python | crafted | The same name twice, a directory replaced by a file and a file replaced by a directory. |
| `hostile-hardlink-escape.tar` | Python | crafted | Hard links to targets outside the extraction directory. |
| `hostile-hardlink-missing.tar` | Python | crafted | A hard link to a member that isn't there, and one to itself. |
| `hostile-longname-dotdot.tar` | Python | crafted | A GNU long name (`L`) that climbs out with `..`. |
| `hostile-longname-huge.tar` | Python | crafted | A 4 GiB GNU long name. |
| `hostile-nul-path.tar` | Python | crafted | A pax `path` with an embedded NUL. |
| `hostile-octal-garbage.tar` | Python | crafted | A size field with a non-octal digit. |
| `hostile-pax-badlength.tar` | Python | crafted | A pax record whose length runs past the end of the header data. |
| `hostile-pax-huge.tar` | Python | crafted | A 1 GiB pax header with one record behind it. |
| `hostile-pax-length.tar` | Python | crafted | A pax record that claims to be 99999999999 bytes long. |
| `hostile-pax-noequals.tar` | Python | crafted | A pax record with no `=`. |
| `hostile-pax-path.tar` | Python | crafted | pax `path` and `linkpath` records that override harmless header fields with escapes. |
| `hostile-size-absurd.tar` | Python | crafted | A size of 8 GiB - 1 with four bytes of data behind it. |
| `hostile-size-base256.tar` | Python | crafted | A base-256 size of 2^63 - 1. |
| `hostile-size-negative.tar` | Python | crafted | A negative base-256 size. |
| `hostile-sparse-map.tar` | Python | crafted | A GNU sparse map with overlapping regions and regions past the real size. |
| `hostile-symlink-absolute.tar` | Python | crafted | A symlink to `/etc`, then a file written through it. |
| `hostile-symlink-chain.tar` | Python | crafted | An escape through a chain of two symlinks, and a symlink to itself. |
| `hostile-symlink-escape.tar` | Python | crafted | A symlink to `../..`, then a file written through it. |
| `hostile-truncated-data.tar` | Python | crafted | A file that ends 600 bytes into a 1000-byte member. |
| `hostile-truncated-header.tar` | Python | crafted | A file that ends 300 bytes into a header. |
| `hostile-windows-names.tar` | Python | crafted | Names Windows reserves or can't hold: devices, an alternate data stream, trailing dots and spaces, backslashes, control and wildcard characters. |

## libarchive and pax

| File | Made with | How | What it exercises |
|---|---|---|---|
| `bsd-mac-default.tar` | macOS 27.0.1 bsdtar 3.5.3 | `tar -cf bsd-mac-default.tar tree` | macOS defaults: restricted pax, with `._README.TXT` and `._hard.txt` AppleDouble entries for the extended attribute, which is stored again as `LIBARCHIVE.xattr` and `SCHILY.xattr` records. |
| `bsd-mac-gnutar.tar` | macOS 27.0.1 bsdtar 3.5.3 | `COPYFILE_DISABLE=1 tar --format gnutar -cf bsd-mac-gnutar.tar tree long` | GNU format from libarchive on macOS. |
| `bsd-mac-pax.tar` | macOS 27.0.1 bsdtar 3.5.3 | `COPYFILE_DISABLE=1 tar --format pax -cf bsd-mac-pax.tar tree long` | pax from libarchive on macOS: atime and ctime, the extended attribute as `LIBARCHIVE.xattr` and `SCHILY.xattr` records, symlinks, a hard link, long names and link targets. |
| `bsd-mac-ustar.tar` | macOS 27.0.1 bsdtar 3.5.3 | `COPYFILE_DISABLE=1 tar --format ustar -cf bsd-mac-ustar.tar tree` | ustar from libarchive on macOS. |
| `bsd-mac-v7.tar` | macOS 27.0.1 bsdtar 3.5.3 | `COPYFILE_DISABLE=1 tar --format v7 -cf bsd-mac-v7.tar tree` | V7 from libarchive on macOS. |
| `bsd-mac.tar.bz2` | macOS 27.0.1 bsdtar 3.5.3 | `tar -cjf` | bzip2 from libarchive. |
| `bsd-mac.tar.gz` | macOS 27.0.1 bsdtar 3.5.3 | `tar -czf` | gzip from libarchive. |
| `bsd-mac.tar.xz` | macOS 27.0.1 bsdtar 3.5.3 | `tar -cJf` | xz from libarchive. |
| `bsd-win-default.tar` | Windows 10 22H2 bsdtar 3.8.9 | `tar -cf bsd-win-default.tar tree` | Windows `tar.exe` defaults: restricted pax, with pax headers only where ustar falls short. |
| `bsd-win-gnutar.tar` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format gnutar -cf bsd-win-gnutar.tar tree long` | GNU format from libarchive on Windows: `././@LongLink` for long names. |
| `bsd-win-pax.tar` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format pax -cf bsd-win-pax.tar tree long` | pax as written by libarchive on Windows: `LIBARCHIVE.creationtime`, atime, ctime and mtime records. |
| `bsd-win-ustar.tar` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format ustar -cf bsd-win-ustar.tar tree` | ustar from libarchive on Windows. |
| `bsd-win-v7.tar` | Windows 10 22H2 bsdtar 3.8.9 | `tar --format v7 -cf bsd-win-v7.tar tree` | V7 from libarchive on Windows. |
| `bsd-win.tar.gz` | Windows 10 22H2 bsdtar 3.8.9 | `tar -z -cf bsd-win.tar.gz tree` | gzip from libarchive on Windows (its only compressor). |
| `pax-mac-tar.tar` | macOS 27.0.1 pax | `pax -w -x tar -f pax-mac-tar.tar tree` | Old (V7-style) tar from the BSD pax. |
| `pax-mac-ustar.tar` | macOS 27.0.1 pax | `pax -w -x ustar -f pax-mac-ustar.tar tree` | ustar from the BSD pax. |

## Notes

- Windows `tar.exe` (bsdtar 3.8.9) crashes with an access violation when it adds a name outside the ANSI code page, so the `bsd-win-*` archives leave out `日本語.TXT` and the symlinks. When reading, it skips such names as "empty or unreadable", and it can't read the GNU multi-volume set or `.lzma`.
- GNU tar warns about the `LIBARCHIVE.*` pax keywords and ignores them.
