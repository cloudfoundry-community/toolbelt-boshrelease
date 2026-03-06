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
    releases:
    - name: toolbelt
      url: https://bosh.io/d/github.com/cloudfoundry-community/toolbelt-boshrelease?v=4.1.0
      version: 4.1.0

The `toolbelt` job sets up all users (present and future) on the
box to source in the appropriate `$PATH`, and `$LD_LIBRARY_PATH`
environment variables, as well as a colorized prompt that shows
you which deployment you are on, what job type, and which
instance.

The `toolbelt-quick` job pulls in a small subset of useful
packages, tuned for a good utility-to-compile-time ratio (hence
the 'quick').


Tools On The Belt
-----------------

The following `toolbelt-*` jobs exist:

- `toolbelt-boss` - The Blacksmith CLI, [boss][boss]
- `toolbelt-cf` - The Cloud Foundry CLI, [cf][cf]
- `toolbelt-cfdot` - The Cloud Foundry Diego Operator Kit, [cfdot][cfdot] (DEPRECATED: archived Sept 2025, moved to diego-release)
- `toolbelt-gaol` - [A CLI for Garden][gaol] (DEPRECATED: unmaintained since 2018)
- `toolbelt-jq` - [jq][jq], it's sed for JSON.
- `toolbelt-mysql-client` - The [MariaDB CLI][mysql].
- `toolbelt-nats` - A utility for interacting with a NATS messagebus.
- `toolbelt-netsniff-ng` - The excellent [netsniff-ng][netsniff-ng] suite of networking diagnostics tools.  **LONG COMPILE TIMES**
- `toolbelt-nload` - [nload][nload] Displays the current network usage
- `toolbelt-nmap` - Network exploration tool and security scanner, [nmap][nmap].
- `toolbelt-psql` - The [PostgreSQL CLI][psql].
- `toolbelt-redis` - The [Redis CLI][redis].
- `toolbelt-safe` - [safe][safe] is an alternate client for Vault.
- `toolbelt-screen` - [screen][screen] Screen is a full-screen window manager that multiplexes a physical terminal between several processes.
- `toolbelt-spruce` - [Spruce][spruce] is a YAML templating tool.
- `toolbelt-tcptrace` - Colorized tcpdump packet captures.
- `toolbelt-tree` - Produce tree-based directory listings.
- `toolbelt-tshark` - Terminal-mode [Wireshark][tshark], for analyzing network protocols at a higher level.  **LONG COMPILE TIMES**
- `toolbelt-vault` - The [Vault CLI][vault], from Hashicorp.

There are some special _meta_-packages that provide subsets of the
above tools, as groups:

- `toolbelt-everything` - Literally, everything.
- `toolbelt-quick` - Just the stuff that compiles quickly (i.e. not netsniff-ng or tshark).


Compilation Times
-----------------

Some packages build from source and take significant time on first
deploy (measured on the Jammy stemcell compilation VM):

| Package            | Compile Time |
|--------------------|--------------|
| toolbelt-tshark    | ~27 min      |
| toolbelt-psql      | ~5 min       |
| toolbelt-netsniff-ng | long       |

Packages that install pre-built binaries (jq, safe, spruce, vault,
boss, etc.) take only seconds to install. BOSH caches compiled
packages, so subsequent deploys reuse the cache unless the package
spec changes.


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
[gaol]:        https://github.com/contraband/gaol
[jq]:          https://jqlang.github.io/jq/
[mysql]:       https://mariadb.org/
[netsniff-ng]: http://netsniff-ng.org/
[nload]:       http://www.roland-riegel.de/nload/
[nmap]:        https://nmap.org/
[psql]:        https://www.postgresql.org/
[redis]:       https://redis.io/
[safe]:        https://github.com/cloudfoundry-community/safe
[screen]:      https://www.gnu.org/software/screen/
[spruce]:      https://github.com/geofffranks/spruce
[tshark]:      https://www.wireshark.org/
[vault]:       https://www.vaultproject.io/
