# codex-window

[![macOS](https://img.shields.io/badge/macOS-only-000000?logo=apple&logoColor=white)](#install)
[![zsh](https://img.shields.io/badge/shell-zsh-4EAA25)](bin/codex-window)
[![License: MIT](https://img.shields.io/github/license/Siwoo4985/codex-window)](LICENSE)

Launch isolated OpenAI Codex desktop profiles from the terminal on macOS.

Codex desktop already supports a normal new window with `Cmd+Shift+N`. Use that
if you only need another window in the same app profile. `codex-window` is for a
different workflow: starting Codex windows from the shell with separate Electron
user data directories, named profiles, per-profile logs, and repeatable launch
commands.

```sh
cw frontend
cw backend
cw 3
```

## Install

Requirements:

- macOS
- OpenAI Codex desktop app installed at `/Applications/Codex.app`
- zsh

Install from GitHub:

```sh
git clone https://github.com/Siwoo4985/codex-window.git
cd codex-window
./scripts/install
```

The installer creates these symlinks in `~/.local/bin`:

```text
codex-window
cw
```

If your shell cannot find `cw`, add `~/.local/bin` to your `PATH`.

## Use

Open one generated isolated profile:

```sh
cw
```

Open several generated isolated profiles:

```sh
cw 3
```

Open one named isolated profile:

```sh
cw frontend
cw --name docs
```

Pass arguments to Codex after `--`:

```sh
cw frontend -- ~/Projects/my-app
```

Use the long command if you prefer:

```sh
codex-window 3
```

## When To Use This

Use Codex's built-in `Cmd+Shift+N` when:

- you just want another normal Codex window
- you want the same login, cache, and local app state
- you prefer the app UI over a terminal workflow

Use `codex-window` when:

- you want named profiles like `frontend`, `backend`, or `docs`
- you want profile data and logs separated by project
- you want to launch several Codex profiles from the terminal
- you want a dry run of the exact `open` command before launching

## Command Reference

```text
cw [count] [-- extra Codex args...]
cw --count <count> [-- extra Codex args...]
cw --name <profile> [-- extra Codex args...]
cw --profile <profile> [-- extra Codex args...]
```

Options:

```text
-c, --count <count>        Open this many generated profiles.
-n, --name <profile>      Open one named profile.
    --profile <profile>   Alias for --name.
    --slot <count>        Compatibility alias for --count.
    --app <path>          Codex.app path. Default: /Applications/Codex.app
    --profile-root <dir>  Profile root directory.
    --dry-run             Print launch commands without opening windows.
-h, --help                Show help.
    --version             Show version.
```

## Profiles And Logs

By default, profile data and logs are stored in:

```text
~/Library/Application Support/Codex-Window/
```

Generated profiles use names like `window-1`, `window-2`, and `window-3`.
Named profiles use the name you pass, such as `frontend` or `docs`.

Override the profile root:

```sh
CODEX_WINDOW_PROFILE_ROOT="$HOME/.codex-window-profiles" cw 2
```

Override the Codex app path:

```sh
CODEX_WINDOW_APP="/Applications/Codex Canary.app" cw 2
```

## How It Works

Codex desktop is an Electron app. `codex-window` launches it with a separate
profile directory:

```text
CODEX_ELECTRON_USER_DATA_PATH=<profile-dir>
open -n /Applications/Codex.app
```

Each profile gets its own Electron user data path. That separates local app
state such as cookies, cache, storage, and logs from the default Codex profile.

## Troubleshooting

Check what would be launched without opening Codex:

```sh
cw --dry-run 2
```

If `cw` is not found, run:

```sh
echo $PATH
```

and make sure `~/.local/bin` is included.

If Codex is installed somewhere else:

```sh
cw --app "/path/to/Codex.app" 2
```

## Notes

- This project is macOS-only.
- This project is not affiliated with OpenAI.
- A new isolated profile may need its own login, depending on your Codex auth
  state.
- Running many Codex profiles can start many helper/app-server processes and use
  significant memory.

## Related Work

- [openai/codex#12773](https://github.com/openai/codex/issues/12773) tracks
  native macOS multi-window support for the Codex desktop app.
- [Lampese/codex-switcher](https://github.com/Lampese/codex-switcher) manages
  multiple Codex CLI accounts.
- [vaportail/codex-windows-updater](https://github.com/vaportail/codex-windows-updater)
  is an unofficial Windows installer/updater for Codex.

## Development

Run syntax checks:

```sh
zsh -n bin/codex-window scripts/install
```

Preview generated launch commands:

```sh
bin/codex-window --dry-run 2
```

## License

MIT
