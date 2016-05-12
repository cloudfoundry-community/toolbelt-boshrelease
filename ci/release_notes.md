## Fixes

- Compiled packages (netsniff-ng, mostly) now take advantage of
  multiple-core compilation VMs by running parallel make with `-j`
  set to the number of cores present.

## New Features

- **tshark** (and other Wireshark CLI utilities) are now included
  in the `toolbelt-tshark` job.  Finally, protocol-aware packet
  analysis in BOSH!
- **tcptrace**, [a colorful tcpdump filter][1], is now included in
  the `toolbelt-tcptrace` job.



[1]: https://github.com/jhunt/tcptrace
