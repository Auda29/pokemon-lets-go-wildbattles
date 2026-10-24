# Implementation roadmap

Milestone 0 is in progress. The unmodified v0.7.1 runtime is prepared locally and its launcher process and initial Vulkan setup were checked. A supplied base NSP is unpacked locally; decrypting its game content is blocked by missing local keys. No feature implementation or gameplay verification has been completed. See the [baseline report](BASELINE_REPORT.md).

## Milestone 0: reproduce the upstream baseline

Current status: source pinned, official Windows package prepared, and base NSP unpacked to NCA files. ExeFS/RomFS extraction and every gameplay check below remain unrun.

- Check out the commit recorded in [upstream research](UPSTREAM_RESEARCH.md) in separate local development storage.
- Follow that revision's build instructions using a locally supplied Eevee 1.0.0 dump.
- Reproduce an ordinary wild battle, CATCH selection, failed throw, opponent turn, successful capture, knockout EXP, and player defeat.
- Compare the encountered Pokemon with the stored Pokemon, including identity and moves.
- Record the actual build, settings, logs, and visible results. Separate upstream bugs from proposed changes.
- Choose a downstream branch or patch-set layout and retain required upstream notices if code is imported.

Exit condition: a reproducible baseline report and a list of verified gaps. An upstream source comment alone does not satisfy it.

## Milestone 1: establish a capture action inside battle

- Identify the battle action dispatch and lifecycle.
- Identify ball inventory and Pokemon storage/Pokedex interfaces.
- Prove that a capture attempt can return to the same active battle without entering the Let's Go capture scene.
- Establish a reliable way to keep the exact encountered Pokemon through capture and storage.
- Research FireRed/LeafGreen's capture algorithm and derive independent expected results.

Exit condition: a documented integration path for failure, success, and cleanup. If existing hooks cannot support it, record the missing runtime changes before proceeding.

## Milestone 2: one complete playable encounter

- One ordinary species and one supported area on Eevee 1.0.0.
- Attack, switch Pokemon, and select a Poke Ball from the battle UI.
- Apply the capture calculation using HP and major status.
- Failed throw consumes one ball and one player action; the opponent acts and all battle state persists.
- Successful throw stores the correct Pokemon once and ends the encounter.
- Knockout grants EXP, supports a level-up, and ends without capture.
- Verify escape, player defeat, and mode disabled using backup saves.

Exit condition: all first-prototype checks in [the test plan](TEST_PLAN.md) pass with logs and visual evidence.

## Milestone 3: complete the supported rules

- Great, Ultra, and Master Balls, with verified inventory behavior.
- Exact integer rounding and deterministic capture tests.
- Multiple participants, EXP distribution, move learning, and evolution.
- Full party, full boxes, no balls, trapping effects, fainting, and interruption handling.
- Shiny Pokemon, forms, identity preservation, and overworld encounter removal.
- Final decisions on Catch Combos, bonuses, special encounters, and escape rules.
- Pikachu 1.0.0 address mapping and regression verification.

Exit condition: the supported gameplay rules are documented and verified on each advertised game/version combination.

## Milestone 4: optional mode and distribution

- Persistent pre-session mode selection and a verified disabled path.
- Polished battle-scene throw/result presentation, supported languages, and controller input.
- Clean setup instructions and a reproducible runtime build or patch process.
- Release notes with tested versions, known issues, and local data requirements.
- Playthrough checks for early progression, trainer battles, scripted encounters, and blackouts.

Exit condition: a reproducible release that works with the user's own game data and includes no game assets or generated game code.

## Later work

Playable Red and Green are a separate feature after the classic encounter mode is stable. Support for 1.0.1/1.0.2 needs its own address research and validation. Neither is promised by the first release.
