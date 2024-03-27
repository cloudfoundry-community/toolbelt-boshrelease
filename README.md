BOSH Toolbelt
=============

**Toolbelt** is an add-on BOSH release that provides a set of
packages that are indispensible in troubleshooting and diagnosing
issues on BOSH deployments.

Yes, yes, conventional wisdom holds that you ought not be SSHing
into your BOSH VMs to do things, but when systems break down,
on-box troubleshooting is often the fastest way to cut through to
the heart of the issue.

But first, you need a Toolbelt.


Using the Toolbelt
------------------

To outfit your BOSH VMs with awesome tools, first upload Toolbelt
to your BOSH director:

    bosh target https://192.168.50.4:25555
    bosh upload-release https://bosh.io/d/github.com/cloudfoundry-community/toolbelt-boshrelease

Then, add the desired `toolbelt-*` templates to your releases:

    instance_groups:
      - name: my-job
        jobs:
        - { release: toolbelt, name: toolbelt }
        - { release: toolbelt, name: toolbelt-quick }
    releases:
      - name: toolbelt
        version: latest

Or if you're looking to use `toolbelt` as a BOSH `add-on` via `runtime-config`:

    addons:
    - name: toolbelt
      jobs:
      - name: toolbelt
        release: toolbelt
      - name: toolbelt-quick
        release: toolbelt
    - name: toolbelt-veritas
      include:
        jobs:
        - name: bbs
          release: diego
        - name: rep
          release: diego
        - name: auctioneer
          release: diego
      jobs:
      - name: toolbelt-veritas
        release: toolbelt
    releases:
    - name: toolbelt
      sha1: 03b0e2136b02819ec59675f3e38edb24de2d92f9
      url: https://bosh.io/d/github.com/cloudfoundry-community/toolbelt-boshrelease?v=3.3.2
      version: 3.3.2

The `toolbelt` job sets up all users (present and future) on the
box to source in the appropriate `$PATH`, and `$LD_LIBRARY_PATH`
environment variables, as well as a colorized prompt that shows
you which deployment you are on, what job type, and which
instance.o

The `toolbelt-quick` job pulls in a small subset of useful
packages, tuned for a good utility-to-compile-time ratio (hence
the 'quick').


Tools On The Belt
-----------------

The following `toolbelt-*` jobs exist:

- `toolbelt-boss` - The Blacksmith CLI, [boss][boss]
- `toolbelt-cf` - The Cloud Foundry CLI, [cf][cf]
- `toolbelt-cfdot` - The Cloud Foundry Diego Operator Kit, [cfdot][cfdot]
- `toolbelt-esuf` - The ElasticSearch Unassigned Fixer,
  [esuf][esuf], for finding and fixigin UNASSIGNED shards on an
  ElasticSearch cluster (i.e. Logsearch)
- `toolbelt-gaol` - [A CLI for Garden][gaol]
- `toolbelt-gotcha` - A small [MitM proxy][gotcha] for debugging
  HTTP APIs that hide behind SSL/TLS.
- `toolbelt-jq` - [jq][jq], it's sed for JSON.
- `toolbelt-nload` - [nload][nload] Displays the current network usage
- `toolbelt-nats` - A utiity for interacting with a NATS
  messagebus.
- `toolbelt-netsniff` - The excellent [netsniff-ng][netsniff-ng]
  suite of networking diagnostics tools.  **LONG COMPILE TIMES**
- `toolbelt-psql` - The [PostgreSQL CLI][psql].
- `toolbelt-redis` - The [Redis CLI][redis].
- `toolbelt-vault` - The [Vault CLI][vault], from Hashicorp.
- `toolbelt-safe` - [safe][safe] is an alternate client for Vault.
- `toolbelt-screen` - [screen][screen] Screen is a full-screen window manager that 
  multiplexes a physical terminal between several processes.
- `toolbelt-tree` - Produce tree-based directory listings.
- `toolbelt-tcptrace` - Colorized tcpdump packet captures.
- `toolbelt-tshark` - Terminal-mode [Wireshark][tshark], for
  analyzing network protocols at a higher level.  **LONG COMPILE
  TIMES**
- `toolbelt-veritas` - The [veritas][veritas] diagnostic tool for
  the [Diego Runtime][diego].

There are some special _meta_-packages that provide subsets of the
above tools, as groups:

- `toolbelt-everything` - Literally, everything.
- `toolbelt-quick` - Just the stuff that compiles quickly (i.e.
  not netsniff, tshark or veritas).


Playing on BOSH-lite
--------------------

You can create a small, working manifest file from this git
repository:

    git clone https://github.com/cloudfoundry-community/toolbelt-boshrelease
    cd toolbelt-boshrelease
    ./templates/make_manifest warden
    bosh -n deploy

Then, you can `bosh ssh` and see what it is like using Toolbelt!


[boss]:        https://github.com/blacksmith-community/boss
[cf]:          https://github.com/cloudfoundry/cli
[cfdot]:       https://github.com/cloudfoundry/cfdot
[esuf]:        https://github.com/starkandwayne/esuf
[gaol]:        https://github.com/contraband/gaol
[gotcha]:      https://github.com/jhunt/gotcha
[jq]:          https://stedolan.github.io/jq/
[netsniff-ng]: http://netsniff-ng.org/
[nload]:       http://www.roland-riegel.de/nload/
[psql]:        http://www.postgresql.org/
[redis]:       http://redis.io/
[vault]:       https://www.vaultproject.io/
[safe]:        https://github.com/jhunt/safe
[screen]:      https://www.gnu.org/software/screen/
[tshark]:      https://www.wireshark.org/
[veritas]:     https://github.com/pivotal-cf-experimental/veritas
[diego]:       https://github.com/cloudfoundry-incubator/diego-release
