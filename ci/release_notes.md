## New Features

Toolbelt now includes [gotcha][gotcha] for all your HTTP proxying
and diagnostic needs!  Because it is a MiTM proxy, it's in a
different job (`toolbelt-dev`) all by itself.  Other tools you may
not want to run in production will also go in there, as time goes
on.

The `toolbelt/errands` job can now be used to build run multiple
other errand tasks, either in parallel or serially, within a
single errand job.  This lets you bundle lots of test errands
together, for example, and not have to deal with BOSH locking when
you want to run them all at once.

Using it is as easy as:

    jobs:
      - name: test-all-the-things
        templates:
          - { release: toolbelt, name: errands }
          - { release: other,    name: smoke-tests }
          - { release: other,    name: acceptance-tests }
          - { release: other,    name: network-tests }
        # other necessary job config

The new `toolbelt.errands.*` properties govern how this behaves.

[gotcha]: https://github.com/starkandwayne/gotcha
