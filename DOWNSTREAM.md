# Downstream maintenance

This fork keeps two long-lived branches:

- `main` is a fast-forward mirror of `PeonPing/peon-ping`'s `main`.
- `downstream` is the installed branch and carries the local patch stack.

The remotes are named `upstream` for `PeonPing/peon-ping` and `origin` for
`lukewang1024/peon-ping`.

## Patch stack

The initial downstream patch stack combines the remote-SSH click-to-focus
change from upstream PR #562 and the local tmux click-to-focus change from
upstream PR #565. Keep reusable contributions on their topic branches and
cherry-pick them into `downstream` so upstream review does not block installs.

The installer, runtime updater, version check, and PowerShell installer all
point back to `origin/downstream`. The downstream `VERSION` suffix is required:
it makes an accidental upstream installation visible immediately.

## Sync upstream

```sh
git fetch upstream
git switch main
git merge --ff-only upstream/main
git push origin main

git switch downstream
git rebase main
bats tests/
git push --force-with-lease origin downstream
```

After merging or rebasing, bump `VERSION` to the next `-downstream.N` value and
rerun the installer from the raw `downstream` URL.
