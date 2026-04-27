# Agent workspace migration (local + GitHub)

## Git remote

- **Repository:** [DomGrieco/agent-workspace](https://github.com/DomGrieco/agent-workspace) (renamed from `my-hermes-workspace`).
- **`git remote`:** `origin` → `https://github.com/DomGrieco/agent-workspace.git`  
- **Upstream** may still be `https://github.com/outsourc-e/hermes-workspace.git` for pull/rebase; remove with `git remote remove upstream` if you no longer track it.

## Private repo limitation

**Public forks cannot be switched to private** on GitHub. To have a *private* repo with the same content:

1. Create a new private repository (e.g. `agent-workspace` under your user).
2. `git push --all` and `git push --tags` to the new remote.
3. Update `origin` to the new URL.
4. (Optional) Delete or keep the public fork; open PRs on the new repo are listed below when applicable.

## Pull request (upstream)

Cross-fork PR for review/merge to upstream (if you contribute back):

- <https://github.com/outsourc-e/hermes-workspace/pull/173>

## Canonical local path

- Clone/working copy: `~/Documents/projects/agent-workspace` (replaces `my-hermes-workspace`).
- Dom helper scripts default `WORKSPACE_DIR` to `~/Documents/projects/agent-workspace` and `HERMES_HOST_PROJECTS` to `~/Documents/projects` in `docker-compose.override.yml`.

## Archive old `hermes-workspace` tree

If an older `~/Documents/projects/hermes-workspace` directory still exists, move it (do not delete) to a timestamped archive:

```bash
mkdir -p ~/Documents/projects/_archive
STAMP=$(date -u +%Y%m%dT%H%M%SZ)
mv ~/Documents/projects/hermes-workspace \
  "$HOME/Documents/projects/_archive/hermes-workspace-$STAMP"
```

## Runtime verification (after `make recreate` or `make rebuild-and-recreate`)

**Do not** use `docker compose down -v` (preserves `.runtime/hermes-data`).

From the repo root:

- `make verify`
- <http://127.0.0.1:3000> (workspace UI)
- `curl -sS http://127.0.0.1:8642/health` (gateway; may require running stack)
- `curl -sS http://127.0.0.1:9119/api/status` (dashboard)
- If Tailscale is enabled in compose, test `http://<HERMES_TAILSCALE_IP>:3000` from the phone (see `.env` / override).

## Open GitHub issues / PRs on the fork (migration checklist)

If you had open PRs on `DomGrieco/my-hermes-workspace` before the rename, GitHub should redirect URLs to `DomGrieco/agent-workspace`. Re-point automation (CI, local clones) to the new name.

## Cursor / IDE

Re-open the workspace at `~/Documents/projects/agent-workspace` so path-based tools and rules match the on-disk location.
