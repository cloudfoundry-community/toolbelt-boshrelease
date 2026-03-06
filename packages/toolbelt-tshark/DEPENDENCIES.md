# toolbelt-tshark Dependency Tree

This file documents the dependency relationships and version choices for
each tshark build. The packaging script builds these in order; the spec
file lists the corresponding blobs.

## Wireshark 4.4.2 (current)

Stemcell: Ubuntu Jammy (22.04)
- glibc 2.35, glib 2.72 (too old for Wireshark 4.x which requires glib 2.76+)

Note: Ubuntu Noble (24.04) stemcell is planned. Noble ships glib 2.80 and
glibc 2.39. Wireshark 4.4.2 requires glib 2.76+, so Noble's system glib
may be sufficient. When migrating to Noble, evaluate whether glib (and its
build chain: meson, ninja, pkgconf, libffi, Python) can be dropped from
this build in favor of the system version.

```
wireshark 4.4.2
├── glib 2.82.2          (>= 2.76 required; Jammy ships 2.72)
│   ├── libffi 3.4.6     (glib subproject dependency)
│   ├── meson 1.10.1     (glib 2.82+ requires meson build system)
│   │   └── Python 3.14.2  (meson is a Python package)
│   │       └── libffi 3.4.6  (Python's ctypes module)
│   ├── ninja 1.13.2     (build executor for meson)
│   └── pkgconf 2.3.0    (library discovery; stemcell has no pkg-config)
├── c-ares 1.34.6        (async DNS resolver)
├── libpcap 1.10.5       (packet capture)
├── libgcrypt 1.11.2     (TLS decryption support)
│   └── libgpg-error 1.58
└── [speexdsp]           (SKIPPED - optional, VoIP audio codec analysis only)
                         (requires autotools/autogen.sh not on stemcell)
```

### Build tools provided by stemcell

gcc, g++, make, cmake, bison, flex, patch

### Build tools NOT on stemcell (built from source)

python3, pip3, meson, ninja, pkg-config (pkgconf)

### Key build decisions

- **glib --libdir**: Meson defaults to Debian multiarch paths
  (`lib/x86_64-linux-gnu/`). The toolbelt envrc only adds `lib/` to
  `LD_LIBRARY_PATH`, so we use `--libdir=${BOSH_INSTALL_TARGET}/lib`
  to install libraries in the expected location.

- **speexdsp skipped**: The git repo source requires `autogen.sh`
  (autotools) which the stemcell does not have. Only needed for
  wireshark/sharkd/logray (GUI/daemon), not tshark CLI. The release
  tarball from https://ftp.osuosl.org/pub/xiph/releases/speex/ includes
  a pre-generated configure script if this is ever needed.

- **BUILD_sharkd=OFF, BUILD_logray=OFF**: These targets require speexdsp.
  Disabling them allows tshark to build without it.

- **pkgconf symlinked as pkg-config**: Meson and cmake look for
  `pkg-config` on PATH. pkgconf is a compatible implementation;
  the symlink makes it discoverable.

- **Python make install (not altinstall)**: `altinstall` only creates
  versioned binaries (`python3.14`, `pip3.14`). We need `pip3` and
  `python3` on PATH for meson installation.
