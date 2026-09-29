<p align="center">
    <a href="https://github.com/lupaxa-git-toolbox">
        <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/organisations/git-toolbox/readme-logo.png" alt="Organisation Logo" />
    </a>
</p>

<h1 align="center">Every Repo</h1>

Run a command, a command file, or a script in every Git repository under a path.

## What it Does

`every-repo` walks a directory tree, finds each Git repository, and runs the same work in each one. A `.git` directory or a `.git` file counts (worktrees and submodules). The walk does not descend into `.git`.

A failure is reported, then the next repository still runs. `--fail-fast` stops at the first failure. By default, no directories are excluded.

Use it when the same Git or Make command needs to land in many checkouts — for example `git status`, `make init`, or a local update script. Use `--list` first when the command is destructive.

## Install

Run the script from a clone:

```bash
./src/every-repo --help
```

## Quick Start

```bash
./src/every-repo . --list
./src/every-repo . --command git status
./src/every-repo . --command make init
./src/every-repo . --file commands.sh
./src/every-repo . --script ./update-repo.sh --dry-run
```

## Default Behaviour

With a root path and one execution mode, the script will:

1. Resolve the root to an absolute path
2. Walk that tree, without entering `.git` or any directory named by `--exclude`
3. Treat each `.git` directory or `gitdir:` file as a repository
4. Run the command, file, or script in each remaining repository
5. Print how many repositories were processed, skipped, excluded, and failed

The starting path is always entered, even when its own name is in `--exclude`.

A non-zero exit from one repository does not stop the walk unless `--fail-fast` is set. The script exits with status 1 if any repository failed.
`--fail-fast` exits with that repository's status. `--fail-empty` exits with status 1 when nothing ran.

## Common Options

| Flag                    | Purpose                                              |
| :---------------------- | :--------------------------------------------------- |
| `-e, --exclude NAME`    | Do not enter a directory with this name (repeatable) |
| `-c, --command CMD ...` | Run an executable in each repository                 |
| `-f, --file FILE`       | Run shell commands from FILE in each repository      |
| `-s, --script SCRIPT`   | Run SCRIPT in each repository; extra args are passed |
| `-l, --list`            | Print matching repositories and exit                 |
| `--fail-fast`           | Stop at the first non-zero exit                      |
| `--fail-empty`          | Exit 1 when no repository ran                        |
| `-V, --version`         | Print version and exit                               |
| `-h, --help`            | Show help                                            |

`--command` does not use a shell. `&&`, pipes, and other shell syntax belong in `--file` or `--script`.

`--file` and `--script` paths are resolved before entering any repository. A relative `--command` path is resolved inside each repository.

```bash
./src/every-repo --help
```

## Examples

List the repositories that would run:

```bash
./src/every-repo . --list
```

Status of every repository under the current directory, stopping at the first failure:

```bash
./src/every-repo . --fail-fast --command git status
```

Run a command file, without entering two directory names:

```bash
./src/every-repo \
  --exclude repo-1 \
  --exclude repo-2 \
  ~/Desktop/GitMaster \
  --file commands.sh
```

`--exclude repo-1` skips that directory and every repository inside it.

Run a script and forward its own flags:

```bash
./src/every-repo . --script ./update-repo.sh --dry-run
```

Exit with status 1 when the path contains no repositories:

```bash
./src/every-repo --fail-empty ~/Desktop/GitMaster --command git status
```

## Environment

The command, file, or script receives:

| Variable           | Value                                   |
| :----------------- | :-------------------------------------- |
| `EVERY_REPO_ROOT`  | Absolute starting path                  |
| `EVERY_REPO_DIR`   | Absolute path of the current repository |
| `EVERY_REPO_NAME`  | Directory name of that repository       |

<a href="https://github.com/the-lupaxa-project">
    <img src="https://raw.githubusercontent.com/the-lupaxa-project/brand-assets/master/logos/components/footer-for-child-orgs.svg" alt="The Lupaxa Project Footer" width="100%" />
</a>
