# push_to_github.py

Interactive helper that commits and pushes the folder it's placed in to a GitHub repo, with logging and a progress bar.

## Requirements

- Python 3
- `git` installed and available on PATH

## Setup

Place `push_to_github.py` in the root of the project folder you want to push (the same folder that contains, or will contain, the `.git` repo).

## Usage

```
python3 push_to_github.py
```

The script will:

1. Check that `git` is available.
2. Initialize a repo (`git init`, branch `main`) if the folder isn't already one.
3. Ask you to pick a detected remote, or enter a new repository URL. Verifies the URL is reachable before continuing.
4. Ask you to pick a detected branch, or enter a new one. Checks out or creates it.
5. Ask for a commit message.
   - If left blank, a message is auto-generated: `(this is automated commit message) <timestamp>`.
6. Stages and commits all changes (`git add -A` + `git commit`). Skips the commit step if there's nothing to commit.
7. Fetches the remote and rebases local commits on top if the remote has moved ahead, so the push doesn't get rejected for being behind.
   - If rebase hits a conflict, it aborts cleanly and tells you to resolve it manually with `git pull --rebase <remote> <branch>`.
8. Pushes with a live progress bar.

## Logging

- Every run writes a detailed debug log to the system temp folder first.
- On finish (success or failure), the log is copied into `<project_root>/logs/`.
- The log path is printed at the end of the run.

## Notes

- The script pins itself to the folder it's run from (`SOURCE_DIR`) — it always operates on that folder's repo, not the current working directory.
- On failure, the script prints the error, finalizes the log, and exits with a non-zero code.
