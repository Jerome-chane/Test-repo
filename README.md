# AI_Analyst — build folder

This folder is the compiled app: a framework-dependent .NET 10 build. It needs the .NET 10 runtime, including ASP.NET Core, and nothing else. The source is not here.

## Update

```
git pull
```

Stop the app first (Settings → Quit). A running exe locks its DLLs.

## Start

Double-click `run.cmd`, or:

```
AI_Analyst.exe --open
```

The UI runs at <http://127.0.0.1:47311> (loopback only). A second launch prints that address and exits.

| Option | Effect |
|---|---|
| `--data <folder>` | Data folder. Default: `..\AI_Analyst_data`, next to this folder, outside the git repo |
| `--port <n>` | UI port for this launch (permanent: Settings) |
| `--open` | Open the UI in the browser |
| `--hidden` | No console window (used by the Startup entry and the daily task) |
| `--no-schedule` | Manual runs only |
| `--selfcheck` | End-to-end check without a model; does not touch your data folder |

## Your data stays outside this folder

The data folder holds settings, repo paths, jobs, run state, logs and your edited definitions, prompts, answer forms, templates and taxonomies: `..\AI_Analyst_data` by default. All of it is markdown (`settings.md`, `repos.md`, `jobs\*.md`, `state\<job>\records.md`, `calls.md`), so you can read it in any editor. Nothing in it is ever committed here. Your library edits are saved there as user copies, and they take precedence over the files in `library\`. A `git pull` updates the shipped versions but never overwrites your copies.

To use another data folder permanently, put its path in a `data-folder.txt` next to `AI_Analyst.exe`. That file is git-ignored.

## Upgrading from 0.1

On the first start of 0.2, the old `.json` settings, repos, jobs and library copies in the data folder are converted to `.md`, and the old files are renamed `*.migrated` (nothing is deleted). The log lists each conversion. Old records and call logs are not converted, because their answers were JSON, so those units run again.

## First night

1. **Settings**: check the Ollama URL and the default model (`qwen-coder`). If brain-kit's daemon still runs, add its `PAUSE` file under hold files, so the two never use Ollama at the same time.
2. **Repos**: add each target folder with a neutral alias.
3. **Jobs → New job**: pick a definition and the repos, then set the windows (`daily 23:00-06:00 nightly`, `fri 20:00 +58h weekend`).
4. **Dry run**, then open the outputs under `AI_Analyst_data\dryrun\…`.
5. **Run now** with *Max model calls per run* = 3. Check the run page: real token counts, valid answers, sensible files. If answers fail their checks, the run page shows them as the model wrote them, with the problems that were sent back.
6. **Settings → Start automatically**: Startup folder or daily task. The schedule only opens windows while the app runs.

## Stop and pause

- **Stop** ends the run at the next model call. Finished units are kept, and the next run resumes there.
- **Pause** holds at the next call boundary and blocks scheduled starts. You can also pause from outside the UI by creating `AI_Analyst_data\PAUSE`.
- **Settings → Quit** closes the app.
