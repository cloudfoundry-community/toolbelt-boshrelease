## Bug Fixes

- Ignore packages named 'busybox' when sourcing `PATH` and
  `LD_LIBRARY_PATH`, since those bin / lib directories often
  contain non-executables for a different architecture, or are
  linked to libs we don't want to use (musl, busybox, etc.)

  This mostly only affects toolbelt on diego brain/cell VMs.
