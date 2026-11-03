# 47692

Minimal reproduction for Renovate's `mise` manager. https://github.com/renovatebot/renovate/discussions/47692

## Current behavior

`mise.toml` pins on minor version, `kubectl = "1.37"`, and `mise.lock` is locked at 1.37.0.

Renovate opens **"Update dependency kubectl to v1.37.2"** (branch `renovate/kubectl-1.x-lockfile`), a lock-only update. The whole diff is:

```diff
 [[tools.kubectl]]
-version = "1.37.0"
+version = "1.37.2"
 backend = "aqua:kubernetes/kubernetes/kubectl"
```

The platform entries still point at the 1.37.0 release:

Installing from that lock succeeds, but installs the wrong binary under the new version's name:

```console
$ git checkout renovate/kubectl-1.x-lockfile
$ MISE_LOCKED=1 mise install
✓ installed 1 tool in 8.8s: kubectl@1.37.2
$ mise exec kubectl@1.37.2 -- kubectl version --client
Client Version: v1.37.0
```

## Expected behavior

The PR updates the `url` and `checksum` of every platform entry to 1.37.2, as a fresh `mise lock` would.

## Environment

- Renovate 44.206.2, `npx -y renovate@44.206.2 --binary-source global --allowed-unsafe-executions mise --token $TOKEN djshen/renovate-reproduction`
- mise 2026.10.25 (macos-arm64)
