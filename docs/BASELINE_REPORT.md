# Upstream baseline report

Date: 2026-10-24.

Overall result: local runtime prepared; gameplay verification is blocked by missing game data. Milestone 0 remains incomplete.

## Baseline identity

- Upstream repository: [raulzrrs/letsgo-recomp](https://github.com/raulzrrs/letsgo-recomp).
- Local source checkout: `local/upstream`, detached at `f68942461ff99f5f693cc543866ef05fc4987c61`.
- Release tag: `v0.7.1`, resolved to the same commit.
- Package: `LetsGoRecomp-0.7.1-windows.zip`, 91,168,623 bytes.
- Archive SHA-256: `4defcf6f7900dd80d5b18c3241b4b02a19fa64e72efbf3fbb047f176e6b8f0b6`.
- Intended game target: Let's Go, Eevee! 1.0.0. No local game dump was supplied or validated.

The release package and source checkout are kept under ignored `local/` storage. No upstream runtime changes were made. The runtime binary was downloaded from upstream; it was not built from source here.

## Completed setup checks

- Checked out the exact recorded upstream commit and confirmed its release tag.
- Downloaded the official Windows release and matched its archive hash to GitHub's published digest.
- Extracted the complete runtime package with its bundled code generator, compiler, and dependencies.
- Started `LetsGoRecomp.exe`. Windows reported a responding process with a nonzero main-window handle and the title `Let's Go Recomp`.
- Read startup log entries reporting Vulkan API 1.4.341 and successful selection of the local GPU, followed by render-queue and buffer initialization.

These observations establish process/window creation and initial Vulkan setup. They do not establish a visually correct launcher, successful game-code generation, game boot, performance, or gameplay correctness.

## Visual verification limit

Windows Computer Use failed with:

```text
Computer Use native pipe is unavailable: failed to connect native pipe:
Das System kann die angegebene Datei nicht finden. (os error 2)
```

The error persisted after a retry and JavaScript-session reset. No launcher screenshot or UI interaction was verified. Process metadata and runtime logs are the available startup evidence.

## Gameplay scenarios

All checks below are **not run**. The user confirmed that no local game data is currently available.

- Ordinary wild battle: contact with an eligible encounter opens a battle against the expected Pokemon.
- CATCH: the command is visible and opens the upstream native capture flow.
- Failed capture: a failed throw returns to battle, restores the opponent's HP/status, and allows its next turn. Independently inspect player state, PP, stat stages, and other effects.
- Successful capture: the Pokemon is stored once, the Pokedex updates, and the encounter ends correctly.
- Knockout EXP: defeating the opponent grants EXP once, including a level-up case, and does not start capture.
- Player defeat: a fully fainted party follows the expected recovery path.
- Encounter identity: the encountered, battled, and stored Pokemon retain matching individual properties.
- Disabled mode: ordinary upstream behavior remains available.

No gameplay behavior has been classified as passing or failing. No upstream gameplay bug has been reproduced by this project.

## Next required input

Provide a local directory with the user's own extracted Eevee 1.0.0 game files. Follow [local setup](LOCAL_SETUP.md) to generate the local game library and create a test save. Then execute the scenarios above and attach reviewed evidence to an updated report.
