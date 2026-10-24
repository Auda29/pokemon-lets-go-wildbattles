# Local upstream baseline setup

This setup runs the unmodified upstream Windows release. It does not implement the project's classic capture mode.

## Prepared local layout

The initial setup on 2026-10-24 created these ignored directories:

```text
local/
  downloads/LetsGoRecomp-0.7.1-windows.zip
  game-input/eevee-base-v0.nsp
  game-input/eevee-base-nca/
  runtime/LetsGoRecomp/LetsGoRecomp.exe
  tools/nstool/nstool.exe
  upstream/
```

`local/upstream` is detached at `f68942461ff99f5f693cc543866ef05fc4987c61`, which is also the `v0.7.1` tag. The runtime is the official release package for that tag. Building the runtime from source was unnecessary for this initial startup check.

To open the prepared launcher from the repository root in PowerShell:

```powershell
& .\local\runtime\LetsGoRecomp\LetsGoRecomp.exe
```

If the launcher is already open, use that window. Game data and generated code will stay under the ignored local runtime directory.

## Recreate the local runtime on another checkout

Download [the official v0.7.1 Windows release](https://github.com/raulzrrs/letsgo-recomp/releases/tag/v0.7.1). The expected archive SHA-256, checked against GitHub's release metadata, is:

```text
4defcf6f7900dd80d5b18c3241b4b02a19fa64e72efbf3fbb047f176e6b8f0b6
```

Verify the archive before extracting it:

```powershell
Get-FileHash -Algorithm SHA256 .\LetsGoRecomp-0.7.1-windows.zip
```

Extract the entire package under `local/runtime`, preserving its bundled DLLs, `lasgen`, `zig`, and `gamelib-src` directories. Obtain the source baseline separately if needed:

```powershell
git clone --filter=blob:none --no-checkout https://github.com/raulzrrs/letsgo-recomp.git local/upstream
git -C local/upstream checkout --detach f68942461ff99f5f693cc543866ef05fc4987c61
```

## Required game input

Gameplay requires the user's own extracted Let's Go, Eevee! 1.0.0 files. The launcher accepts a folder containing `exefs` and either `romfs` or `romfs.bin`. Packed XCI/NSP files must be extracted separately first. This repository provides no game files, keys, or saves.

The pinned upstream build detection identifies Eevee 1.0.0 using both of these values:

- Program ID in `main.npdm`: `0100187003A36000`.
- First eight module-ID bytes at offset `0x40` in `exefs/main`: `0a3f0d21e67dad80`.

These are identifiers from upstream's `runtime/gamesetup.c`, not a validation result for the local game package. A subsequently supplied base NSP was unpacked to encrypted NCA files. Its metadata declares the expected Application ID and package version 0, but ExeFS/RomFS decryption is blocked by missing local `prod.keys`. The user's own Switch-derived keys or an already extracted game directory are required to continue. Keep key values out of chat and public Git history.

In the launcher:

1. Choose **Select game folder...** and select the extracted game directory.
2. Wait for file preparation and local game-code generation/compilation to complete.
3. Confirm the game/build is supported before enabling experimental options.
4. Use the original 30 fps setting, disable frame generation, and leave translation/file mods disabled for the baseline.
5. Start a new game and progress until ordinary wild encounters, balls, and the partner-play menu are available. Keep wild battles disabled during this preparation.
6. Exit cleanly and back up the generated save folder before testing.

No pre-existing save is required, but reaching a suitable test location is part of the first gameplay session. Upstream's introductory tutorials and early progression also need verification.

## Run the baseline checks

Use a fresh copy of the prepared save for each scenario. Keep the original backup unchanged. The runtime normally stores saves under `local/runtime/LetsGoRecomp/save`; its `LARECOMP_SAVE_DIR` diagnostic option can point test runs at separate copies.

Enable **Wild battles** on the launcher's Game page for the wild battle cases. Start with natural gameplay and no forced species, HP, capture rates, or automated CATCH requests.

For diagnostic runs, these upstream variables add event logs without intentionally changing capture outcomes:

```powershell
$env:LARECOMP_WILDBATTLE_STATE_LOG = '1'
$env:LARECOMP_WILDBATTLE_CATCHLOG = '1'
$env:LARECOMP_WILDBATTLE_HPLOG = '1'
& .\local\runtime\LetsGoRecomp\LetsGoRecomp.exe
```

The variables apply to this PowerShell session and child processes. Close the session after testing, or remove the three variables before an ordinary run.

Follow the scenario list in [the baseline report](BASELINE_REPORT.md). Record the visible result and corresponding log events. Preserve local evidence under `local/`; publish only reviewed, sanitized findings. A capture retry log alone does not establish that all battle state survived.

## Current limit

Only launcher startup and Vulkan initialization were observed during initial setup. Game-data preparation, local game-code compilation, and every gameplay scenario remain unrun. See [the dated report](BASELINE_REPORT.md) for exact status.
