# dotnet-scripts

Handy helpers for managing a .NET SDK install on macOS and Linux development machines.

These scripts use Microsoft's `dotnet-install.sh` installer and manage SDKs under `~/.dotnet`.
If Homebrew manages your .NET installation, the scripts will defer to `brew`.

## Scripts

### dotnet-update

Updates or installs .NET using either Homebrew (if detected) or the Microsoft installer.

No existing .NET installation is required. To install your first SDK into `~/.dotnet`, run:

```bash
./dotnet-update --channel 11.0
```

The `--all` option requires an existing SDK installation with `dotnet` on PATH so it can
discover the installed channels.

Usage:

```bash
./dotnet-update --channel <value> [--quality <value>]
./dotnet-update --all
```

Options:
- `--all`: Update every installed SDK channel. Channels with a stable SDK update to the latest
  `GA` build; channels containing only prerelease SDKs update to the latest `preview` build.
- `--channel <value>`: Install from a numbered channel (for example: `11.0` or `11.0.1xx`).
- `--quality <value>`: Install the latest `GA`, `preview`, or `daily` build in the specified
  channel. This requires `--channel`.

Specify either `--channel` or `--all`; running without arguments prints usage followed by
`dotnet --info` and exits with code `1`. If `dotnet` is not on PATH, it prints a message instead.
Use `--help` to print only usage and exit with code `0`.
The `--lts` and `--sts` options are no longer supported. Choose a numbered channel explicitly
to control major-version upgrades without relying on Microsoft's LTS/STS download aliases.

For example, install the latest SDK in the .NET 11 channel:

```bash
./dotnet-update --channel 11.0
```

For example, install the latest public .NET 11 preview:

```bash
./dotnet-update --channel 11.0 --quality preview
```

Update every installed SDK channel, including preview-only channels:

```bash
./dotnet-update --all
```

Notes:
- If Homebrew is detected, this uses `brew upgrade` for the installed `dotnet`/`dotnet-sdk` formula or cask.
- `--all` is only supported for Microsoft-installer-managed SDKs.
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
- Prompts for one or more versions to remove. Enter selections separated by spaces, such as `1 2 4 5`.
- Validates the full selection and asks for one confirmation before removing the selected directories.
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

## License

This project is available under the [MIT License](LICENSE).
