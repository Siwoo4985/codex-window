# codex-window

Open multiple independent OpenAI Codex desktop windows on macOS.

`codex-window` is a tiny zsh launcher for `Codex.app`. It starts each window
with a separate Electron user data directory via `CODEX_ELECTRON_USER_DATA_PATH`,
which avoids the single-instance lock that normally brings the existing Codex
window to the front.

## Why

The Codex desktop app currently has an open macOS feature request for native
multi-window support. Until that lands, this launcher gives each window its own
profile so separate Codex instances can run side by side across projects,
monitors, or macOS Spaces.

## Install

```sh
git clone https://github.com/Siwoo4985/codex-window.git
cd codex-window
./scripts/install
```

The installer creates two symlinks in `~/.local/bin`:

```text
codex-window
cw
```

Make sure `~/.local/bin` is on your `PATH`.

## Usage

Open one new Codex window:

```sh
cw
```

Open several new windows:

```sh
cw 3
```

Open one named profile:

```sh
cw frontend
cw --name docs
```

Pass extra arguments to Codex after `--`:

```sh
cw frontend -- ~/Projects/my-app
```

Use the long command if you prefer:

```sh
codex-window 3
```

## Profiles

By default, profile data and logs are stored in:

```text
~/Library/Application Support/Codex-Window/
```

Each generated window uses a profile like `window-1`, `window-2`, and so on.
Named profiles use the name you pass.

Override the profile root:

```sh
CODEX_WINDOW_PROFILE_ROOT="$HOME/.codex-window-profiles" cw 2
```

## Options

```text
-c, --count <count>        Open this many new Codex windows.
-n, --name <profile>      Open one named profile.
    --profile <profile>   Alias for --name.
    --app <path>          Codex.app path. Default: /Applications/Codex.app
    --profile-root <dir>  Profile root directory.
    --dry-run             Print launch commands without opening windows.
-h, --help                Show help.
    --version             Show version.
```

`--slot <count>` is accepted as a compatibility alias for `--count`, but the
short form is usually clearer:

```sh
cw 10
```

## Notes

- This is macOS-only.
- This is not affiliated with OpenAI.
- Each window has a separate Electron app data profile. Depending on how Codex
  stores auth on your machine, a new profile may need its own login.
- Running many Codex windows can start many helper/app-server processes and use
  significant memory.

## Related Work

I did not find an existing open-source project focused on this exact macOS
Codex.app multi-window launcher use case. Related but different projects:

- [openai/codex#12773](https://github.com/openai/codex/issues/12773) tracks
  native macOS multi-window support for the Codex desktop app.
- [Lampese/codex-switcher](https://github.com/Lampese/codex-switcher) manages
  multiple Codex CLI accounts.
- [vaportail/codex-windows-updater](https://github.com/vaportail/codex-windows-updater)
  is an unofficial Windows installer/updater for Codex.

## Development

Run a syntax check:

```sh
zsh -n bin/codex-window scripts/install
```

Preview launch commands without opening Codex:

```sh
bin/codex-window --dry-run 2
```

## License

MIT
