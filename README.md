# service-hex-ops

A hand-dispatched workflow for Hex package operations that no single package repository owns: retire, unretire and one-off publish.

## What it is for

Each package repository publishes its own releases. This one covers what those workflows do not: retiring a version of a package whose source repository is archived, undoing a retirement, or publishing from another repository's checkout without wiring CI there. It authenticates with the organisation's Hex API key secret.

## Building and running

```sh
gh workflow run hex-ops.yml
```

The workflow file lists the inputs each operation takes.

## Licence

MIT. See [LICENSE](LICENSE).
