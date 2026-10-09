# viva-tracker

A simple tracker for viva sessions. It builds a checklist from the folders in each student's repository. The teacher ticks a question once it is done, and the progress bar and the tick log update automatically.

## How to use

**Teacher**
1. Open the Issues tab of this repository.
2. Open the issue named after the student (`username/repo`).
3. Tick the checkbox of every question that has been covered in the viva.
4. Reload the page after 10 to 20 seconds to see the updated progress bar.

**Student**
1. Keep your repository public.
2. Get your `username/repo` added to `students.txt`.
3. New folders or code files you push will appear in the checklist within 15 minutes.

## Files

| File | Purpose |
|---|---|
| `students.txt` | One `username/repo` per line. An issue is created for every repository listed here. Lines starting with `#` are ignored. |
| `.github/workflows/tracker.yml` | The whole automation. The `sync` job scans the student repositories and creates or updates their issues. The `log` job detects the teacher's ticks, records them in `progress.json` and refreshes the progress bar. |
| `progress.json` | Record of every tick: which question, who ticked it, and when. Updated by the bot, do not edit by hand. |

## Setup

1. Create this repository under your account with the name `viva-tracker`.
2. Go to Settings > Actions > General > Workflow permissions, select **Read and write permissions** and save.
3. Go to Settings > Collaborators and add the teacher with the **Write** role.
4. Open the Actions tab, select Tracker and click **Run workflow**.

## Notes

- Student repositories must be public, otherwise they are skipped.
- Questions are detected automatically from code files (C, C++, Python, Java, JS, TS, Go, Rust and more). A folder containing code becomes one item, and a code file in the repo root becomes its own item.
- If all code sits inside a single parent folder, that folder is opened and its children are listed instead.
- Folders like `node_modules`, `venv`, `build`, `dist` and dot folders are ignored.
- Earlier ticks are kept in `progress.json`, so they are never lost when new folders are added.
- Each tick is committed with a message like `username/repo: Q05 ticked by teacher at 12 Oct 2026, 11:43 AM IST`.