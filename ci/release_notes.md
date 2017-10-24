# New Tools

- `hatop` is a top-like heads-up display for haproxy
  that takes advantage of statistics made available over
  a loopback TCP port, or a UNIX domain socket, via the
  `stats` configuration directive.  It shows useful things like
  backend states, pool load, and more!
- `cfdot` is the new Diego Operator Toolkit, a cli that helps
  you understand what's going on in your diego deployment and
  make changes directly against a running system. For example,
  cfdot give you the ability to directly manage tasks, LRPs,
  locks, etc, all without the need for a fully functional cf
  cloud controller stack.

