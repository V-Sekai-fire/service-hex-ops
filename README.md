<!-- SPDX-License-Identifier: MIT -->
<!-- Copyright (c) 2026 K. S. Ernest (iFire) Lee -->

# hex-ops

Manual GitHub Actions workflow for Hex operations that don't belong to any
one package's own repo — retiring a package whose source repo has been
folded elsewhere and archived (`taskweft_mcp`, `taskweft_plans` →
`taskweft/taskweft`), unretiring, or a one-off publish without needing
repo-specific CI wiring.

Each actively-maintained package repo (`taskweft/nif`, `oauth-mcp-bridge`,
`mcp-client`, `rebac`, ...) keeps its own tag-triggered `publish.yml` for
its *own* releases — this repo doesn't replace that, it covers the
operations those workflows don't.

## Usage

Run via the Actions tab (`workflow_dispatch`) or `gh workflow run`:

```sh
# Retire a version
gh workflow run hex-ops.yml -f operation=retire -f package=taskweft_mcp \
  -f version=0.3.0-dev.2 -f reason=deprecated \
  -f message="Folded into taskweft/taskweft; github.com/taskweft/mcp is archived."

# Unretire
gh workflow run hex-ops.yml -f operation=unretire -f package=taskweft_mcp -f version=0.3.0-dev.2

# Publish from another repo's checkout
gh workflow run hex-ops.yml -f operation=publish -f repository=taskweft/nif -f ref=main
```

`retire`/`unretire` operate directly on hex.pm's registry by package name —
no source checkout needed, since `mix hex.retire` is a Hex archive task, not
project-specific. `publish` checks out `repository`@`ref` and runs
`mix hex.publish --yes` from there, so `mix.exs`'s own version is what gets
published (bump it in that repo first).

Auth is the org-level `HEX_API_KEY` secret (visibility: all repos) — no
per-repo secret setup needed.
