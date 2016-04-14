## New Features

- The `toolbelt.envrc` manifest property can now be used to inject
  arbitrary startup commands, function definitions and prompt
  munging into your session.


## Bug Fixes

- The colorized prompt now properly escapes non-printing sequences
  of ANSI escape codes with `\[` and `\]` so that GNU readline
  will properly calculate available space.  This fixes several
  weird (and very annoying) line-wrapping bugs.
