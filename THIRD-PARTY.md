# Third-party notices

Display 0.0.3 includes applicable license texts under `app/licenses` in each installation archive. Karl’s material is governed by [LICENSE.md](LICENSE.md); third-party terms apply independently and earlier GPL grants remain effective.

| Package | Included components |
| --- | --- |
| R36 ARM64 | CPython 3.11.18, paho-mqtt 2.1.0, Nuitka 2.8.9 runtime helpers; libbz2, OpenSSL, libffi, liblzma, libuuid, libzstd and CPython extension modules |
| TrimUI ARM64 | CPython 3.11.15 from python-build-standalone 20260728, paho-mqtt 2.1.0, websocket-client 1.8.0, Nuitka 2.7.16 runtime helpers and a musl-based application entry sequence |

Paho is distributed under its Eclipse Distribution License 1.0 option. websocket-client and Nuitka runtime helpers use Apache 2.0. R36’s `libuuid` uses its file-specific BSD terms, `libzstd` uses its BSD option, and `liblzma` uses 0BSD. Exact R36 system-library versions are in `app/licenses/r36/runtime-versions.json`; the complete copyright records preserve the file-specific terms.

Startup and compiler notices retain the applicable glibc linking permissions, GCC runtime exception and musl license. TrimUI’s application entry uses the permissive musl sequence instead of GNU crt1. Chromium, graphics libraries, system glibc, fonts and firmware are supplied separately by the device or operating-system repositories and keep their own terms. Terser 5.46.1 is a build tool used to process first-party JavaScript.
