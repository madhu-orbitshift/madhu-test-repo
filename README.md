# Madhu Test Repo

## GitHub Action: Tag Master with Timestamp

This repository includes a GitHub Actions workflow at
`.github/workflows/release-with-timestamp.yml` that creates timestamped tags
and an accompanying GitHub Release.

Key details
- Purpose: automatically create a tag named `v_YYYY.MM.DD.HH.MM.SS` (UTC)
	and create a Release with auto-generated release notes.
- Trigger: `workflow_dispatch` (manual run). You can trigger it from the
	Actions tab. (The workflow can be extended to run on pushes if desired.)
- Runner: `ubuntu-latest`.
- Permissions: requires `contents: write` so the job can push tags and create
	releases.

What the workflow does (high level)
- Generates a UTC timestamp and forms a tag name (for example
	`v_2025.11.10.08.05.09`).
- Creates an annotated Git tag and pushes it to the repository.
- Creates a GitHub Release for that tag and lets GitHub auto-generate the
	release notes from commits/PRs.

Authentication and tokens
- The workflow uses the repository-provided `secrets.GITHUB_TOKEN`.
	The workflow was updated to provide the token to the `gh` CLI via the
	`GH_TOKEN` environment variable (recommended for non-interactive CI use).
- If you need to run the same commands locally you should create a Personal
	Access Token (PAT) with appropriate scopes (repo and/or workflow) and run
	`gh auth login --with-token` locally or set `GITHUB_TOKEN`/`GH_TOKEN` in
	your shell before running equivalent commands.

Notes and tips
- The workflow currently targets the `main` branch when creating the
	release. If your default branch is `master` or different, change the
	`--target` argument or the workflow trigger accordingly.
- Avoid printing secrets in logs. The workflow uses `GH_TOKEN` without
	calling `gh auth login` to prevent the gh CLI from trying to persist
	credentials during the run.

How to trigger
- From the GitHub web UI: go to the Actions tab, select "Tag Master with
	Timestamp", and click "Run workflow".
- Automatic tagging is enabled on push, add a `push` trigger to the workflow
	(for example: `push: branches: [ main ]`).

If you'd like, I can also:
- Commit the workflow changes I made to the repo and push them for you.
- Replace the `gh` CLI steps with an Actions-based release action (for
	example `softprops/action-gh-release` or `ncipollo/release-action`) to avoid
	needing the CLI at all.

