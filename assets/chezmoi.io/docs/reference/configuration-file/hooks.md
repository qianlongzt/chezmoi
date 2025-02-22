# Hooks

Hook commands are executed before and after events. Unlike scripts, hooks are
always run, even if `--dry-run` is specified. Hooks should be fast and
idempotent.

The following events are defined:

| Event                 | Trigger                                       |
| --------------------- | --------------------------------------------- |
| *command*, e.g. `add` | Running `chezmoi command`, e.g. `chezmoi add` |
| `git-auto-commit`     | Generating an automatic git commit            |
| `git-auto-push`       | Running an automatic git push                 |
| `read-source-state`   | Reading the source state                      |

Each event can have a `.pre` and/or a `.post` command. The *event*.`pre` command
is executed before *event* occurs and the *event*`.post` command is executed
after *event* has occurred.

A command contains a `command` or `script` and an optional array of strings
`args`. `command`s are executed directly. `script`s are executed with configured
interpreter for the script's extension, see the [section on
interpreters](interpreters.md).

!!! example

    ```toml title="~/.config/chezmoi/chezmoi.toml"
    [hooks.read-source-state.pre]
    command = "echo"
    args = ["pre-read-source-state-hook"]

    [hooks.apply.post]
    command = "echo"
    args = ["post-apply-hook"]

    [hooks.add.post]
    script = "post-add-hook.ps1'
    ```

When running hooks, the `CHEZMOI=1` and `CHEZMOI_*` environment variables will
be set. `CHEZMOI_COMMAND` is set to the chezmoi command being run,
`CHEZMOI_COMMAND_DIR` is set to the directory where chezmoi was run from, and
`CHEZMOI_ARGS` contains the full arguments to chezmoi, starting with the path to
chezmoi's executable.
