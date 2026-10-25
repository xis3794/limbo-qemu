# limbo-qemu

Downloadable **QEMU emulator cores** for [Limbo for Android](https://github.com/xis3794/limbo).

The Limbo app ships one built-in QEMU build. This repository publishes additional
versions that can be installed from inside the app (**Settings → Version manager**)
without reinstalling the APK.

## Packages

Every release asset is a `tar.xz` archive:

```
qemu-<version>-arm64-v8a.tar.xz
├── manifest.json          version / abi / load order
└── lib/
    ├── libglib-2.0.so     private glib copy (if needed by this build)
    ├── libpcre2-8.so
    ├── libintl.so
    └── libqemu-system-x86_64.so
```

`manifest.json`:

```json
{
  "id": "qemu-11.1.2-arm64-v8a",
  "version": "11.1.2",
  "series": "11",
  "display_name": "QEMU 11.1.2",
  "description": "QEMU 11.1.2 x86_64 emulator core for arm64-v8a devices",
  "abi": "arm64-v8a",
  "qemu_lib": "libqemu-system-x86_64.so",
  "load_order": ["libpcre2-8.so", "libintl.so", "libglib-2.0.so", "libqemu-system-x86_64.so"]
}
```

## Catalog

The app downloads [`catalog.json`](catalog.json) from the `main` branch. Each
entry contains the version, download URL, size and SHA-256 checksum; the app
verifies the checksum before extracting the archive.

Published builds:

| Version | Series | ABI | Release |
|---------|--------|-----|---------|
| 11.1.2  | 11     | arm64-v8a | `qemu-11.1.2` |
| 10.2.0  | 10     | arm64-v8a | `qemu-10.2.0` |
| 8.0.5   | 8      | arm64-v8a | `qemu-8.0.5` |
| 5.1.0   | 5      | arm64-v8a | `qemu-5.1.0` |

## Building

`.github/workflows/qemu-packages.yml` builds each version from the Limbo build
system with NDK r23b (`arm64-v8a`, `x86_64-softmmu`), packages it with
`tools/package_qemu.py` from the Limbo repository, publishes a GitHub release and
updates `catalog.json`.

Run the workflow manually from the **Actions** tab; it can be re-run whenever a
new QEMU version is added to the matrix.
