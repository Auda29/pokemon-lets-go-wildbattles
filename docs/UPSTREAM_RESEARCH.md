# Upstream research

Checked on 2026-10-24. Evidence consists of public documentation and source inspection. No game was built or run for this repository setup.

## Reproducible baseline

- Repository: [raulzrrs/letsgo-recomp](https://github.com/raulzrrs/letsgo-recomp)
- Inspected commit: [`f68942461ff99f5f693cc543866ef05fc4987c61`](https://github.com/raulzrrs/letsgo-recomp/commit/f68942461ff99f5f693cc543866ef05fc4987c61)
- Runtime license: MIT, subject to upstream and third-party notices.

Use this commit when reproducing the initial findings. Upstream main and release packages may differ.

## Documentation baseline

The [pinned README](https://github.com/raulzrrs/letsgo-recomp/blob/f68942461ff99f5f693cc543866ef05fc4987c61/README.md) describes a native Windows port built by static recompilation. Its documented wild battle hooks target Eevee/Pikachu 1.0.0; updates 1.0.1 and 1.0.2 are accepted without those options. It lists LayeredFS file replacement and excludes executable mods such as exefs, IPS, and cheats. It also lists wild battle EXP splitting as unfinished.

Recompilation makes translated game functions callable through runtime hooks. It does not provide a fully reconstructed, named Pokemon game codebase.

## Existing implementation

The [wild battle runtime](https://github.com/raulzrrs/letsgo-recomp/blob/f68942461ff99f5f693cc543866ef05fc4987c61/runtime/hle/wildbattle.c) intercepts ordinary encounters and uses the static encounter machinery. Capture requests leave battle for the native capture scene. Its state tracks opponent HP/status and schedules another battle after a failed throw, including an opponent turn. The file also registers an EXP eligibility hook and resolves hook addresses through `rt_addr`.

The [capture button implementation](https://github.com/raulzrrs/letsgo-recomp/blob/f68942461ff99f5f693cc543866ef05fc4987c61/runtime/hle/catch_button.c) constructs a fifth battle command using the player's local files and serves the modified menu during the mode's battles. It is a useful entry point for UI integration; the presence of that button does not establish a capture action within the battle engine.

The later entries in the [battle research notes](https://github.com/raulzrrs/letsgo-recomp/blob/f68942461ff99f5f693cc543866ef05fc4987c61/docs/MODO-BATALHA.md) describe knockout EXP, timer removal, evolution handling, and capture retries. They also list unresolved identity fields and transition delays. Those notes combine older plans and newer updates, so confirm each claim against source and runtime behavior.

The README's brief description understates this source baseline. Earlier project discussion should not be read as evidence that HP/status modifiers, retries, or knockout EXP must all be invented from scratch.

## Switch reference

[Wild Battles (1.0.2 versions)](https://gamebanana.com/mods/729162), by Rico-Santos, is a separate Switch mod. Its description was retrieved through the [GameBanana API](https://gamebanana.com/apiv11/Mod/729162?_csvProperties=_sName,_sText).

The author describes battle followed by native catching. Listed limitations include no ball use during battle, EXP requiring a catch, and a remaining timer. Reported problems include an early-game softlock before the partner-play menu unlocks and incorrect recovery after the player's team faints.

Use these as regression scenarios, not as evidence that identical bugs exist in Recomp. No mod files or game assets have been copied here. This research does not establish shared implementation or authorship between the mod and Recomp.

## Proposed integration and unknowns

Build on the existing runtime hooks and investigate an action that resolves capture without destroying the active battle. Decide between a downstream branch and a small patch set after the baseline has been reproduced. Ordinary LayeredFS installation is not an established delivery method for this feature.

Research still needs to identify the battle action dispatcher, inventory transaction, capture result presentation, storage/Pokedex handoff, full encounter identity, reward handling, and safe encounter teardown. Confirm each address for each supported game build. Exact function names and layouts beyond those observed above remain unverified.

## Gameplay reference

[Pokemon's official FireRed/LeafGreen description](https://www.pokemon.com/us/pokemon-video-games/pokemon-firered-version-and-pokemon-leafgreen-version) establishes the classic Kanto adventure used as the design reference. The exact capture and EXP algorithms still need primary implementation research. Reference selection is a project decision, not a claim that Recomp already uses those rules.
