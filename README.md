# Git Scripts: Single-Commit Feature Branch Workflow

This guide describes a workflow to keep feature branches as a single commit,
inspect branch changes as staged diffs, and push safely.

## Key Commands

- `git-uncommit-to-staged`
  - Resets the current branch to the fork point and stages all changes.
  - Lets you review everything as uncommitted, tracked changes in your IDE.
- `git-squash-commit`
  - Squashes all branch changes into a single commit on top of the base ref.
  - Prompts to reuse the existing commit message or enter a new one.
- `git-rebase-squash`
  - Rebases onto the base ref and then squashes to a single commit.
  - Useful when you need the latest base changes before squashing.

Each script supports `-h`, `--help`, and `-help` for full usage.

## Typical Workflow

1. Create a feature branch from `develop` and make commits as usual.
2. When you want to inspect all branch changes as staged:
   - `git-uncommit-to-staged --base develop`
3. Review staged changes in your IDE (`git diff --cached`), edit files, and
   stage any updates (`git add -A`).
4. Create a single commit:
   - `git-squash-commit --reuse-message`
   - or `git-squash-commit --message "Your commit message"`
5. If you need the latest base changes first:
   - `git-rebase-squash --base develop`
   - Resolve conflicts if any, then rerun with `--squash-only`.

## Commit Message Handling

- If you do not pass `--message` or `--reuse-message`, the script prints the
  current HEAD message and asks whether to reuse it.
- If you choose not to reuse it, you are prompted for a new message.
- Use `--yes` to auto-accept prompts (reuses the current message when present).

## Remote Safety and Push Behavior

- Remote modification is opt-in:
  - `git-rebase-squash` and `git-squash-commit` only push when `--push` or
    `--push-with-lease` is provided.
  - The dedicated push wrappers require `--yes` to run.
- When a push is enabled, scripts list remotes and warn about history rewrite.

## Recovery Notes

- These workflows rewrite local history. If you need to recover:
  - `git reflog`
  - `git reset --hard <ref>`
