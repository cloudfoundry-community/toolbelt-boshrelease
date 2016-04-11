# New Features

- The [veritas][veritas] utility is now included!
  Thanks to @geofffranks.
- Each package is now managed by its own job, so that if you only
  want (for example) gaol, nats and jq, you don't have to compile
  any other utilities.  You can still get the old behavior by
  specifying the `toolbelt-everything` job.
- By default, adding the base `toolbelt` job to a deployment
  causes the root user, and all users provisioned by `bosh ssh` to
  pick up the necessary paths.
