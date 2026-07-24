# dotnet-scripts

Handy helpers for managing a .NET SDK install on macOS and Linux development machines.

These scripts assume you install .NET via the Microsoft `dotnet-install.sh` script (typically under
`~/.dotnet`). If Homebrew manages your .NET installation, the scripts will defer to `brew`.

## Scripts

### dotnet-update

Updates or installs .NET using either Homebrew (if detected) or the Microsoft installer.

Usage:

```bash
./dotnet-update [--lts | --sts | --channel <value>]
```

Options:
- `--lts` (default): Install the latest Long-Term Support release.
- `--sts`: Install the latest Short-Term Support release.
- `--channel <value>`: Pass an explicit channel to `dotnet-install` (for example: `Current`, `9.0`, `preview`).

Notes:
- If Homebrew is detected, this uses `brew upgrade` for the installed `dotnet`/`dotnet-sdk` formula or cask.
- For Microsoft installs, it ensures `DOTNET_ROOT` and `PATH` are exported in `~/.zprofile` and `~/.zshrc`.

### dotnet-remove

Removes a .NET SDK or runtime installed via `dotnet-install.sh` (under `~/.dotnet` by default).

Usage:

```bash
./dotnet-remove
./dotnet-remove --sdk <version>
./dotnet-remove --runtime <name> <version>
```

Interactive flow:
- Detects installed SDKs and runtimes via `dotnet --list-sdks` / `dotnet --list-runtimes`.
- Prompts for SDK vs runtime.
- Prompts for the version to remove.
- Repeats until you choose to stop.

Non-interactive examples:

```bash
./dotnet-remove --sdk 8.0.100
./dotnet-remove --runtime Microsoft.NETCore.App 8.0.10
```

Notes:
- If Homebrew is detected, it exits and asks you to use `brew uninstall`.
- Only the selected SDK/runtime directory is removed. Other data (like `packs/` or tool caches)
  may remain under `~/.dotnet`.

## Requirements

- Bash (macOS default Bash 3.2 is supported).
- `dotnet` on your PATH for `dotnet-remove`.
- `curl` for `dotnet-update` when using the Microsoft installer.

## Setup

Make the scripts executable:

```bash
chmod +x dotnet-update dotnet-remove
```

Optionally add this repo to your PATH or symlink the scripts into a bin directory.
