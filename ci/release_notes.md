## New Features

- The `toolbelt.envrc` manifest property can now be used to inject
  arbitrary startup commands, function definitions and prompt
  munging into your session.
- **Vault** CLI is now packaged and can be made available by
  deploying either `toolbelt-everything` or the new
  `toolbelt-vault` job.
- **Safe** (an alternate CLI for Vault that provides higher-level
  operations) is now packaged in both `toolbelt-everything` and
  `toolbelt-safe`.


## Bug Fixes

- The colorized prompt now properly escapes non-printing sequences
  of ANSI escape codes with `\[` and `\]` so that GNU readline
  will properly calculate available space.  This fixes several
  weird (and very annoying) line-wrapping bugs.
