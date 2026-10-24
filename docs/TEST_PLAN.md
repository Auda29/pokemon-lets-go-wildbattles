# Verification plan

Status: planned checks. No gameplay checks have been performed for this repository yet.

## Evidence requirements

For each tested build, record the project commit, upstream commit, game edition/version, mode, relevant settings, and result. Use backup saves. Keep saves, dumps, generated code, and raw memory captures out of the public repository.

Use logs to establish actions and state changes, and screenshots or a recording to establish visible behavior. A successful compile cannot verify gameplay. Mark tests as passed, failed, or not run; do not infer a pass from source inspection.

## First prototype acceptance

- [ ] Contact with an ordinary wild Pokemon starts the intended battle.
- [ ] The opponent matches the encountered individual.
- [ ] Attacks change HP and can apply a major status condition.
- [ ] A Poke Ball can be selected from the battle UI; cancelling is free.
- [ ] A committed throw consumes exactly one ball and the player's action.
- [ ] The throw resolves visibly in the battle scene, without entering the Let's Go capture scene.
- [ ] A failed throw gives the opponent its turn and returns to normal action selection.
- [ ] Failed throws preserve player/opponent HP, status, PP, active Pokemon, stat stages, volatile effects, and counters.
- [ ] A successful catch stores the same Pokemon once and updates the Pokedex.
- [ ] A caught or defeated encounter cannot be triggered again from the same overworld object.
- [ ] Knocking out the opponent grants EXP exactly once, including a level-up case.
- [ ] Capturing grants no classic-mode EXP or throw-quality bonus.
- [ ] Escape returns control to the overworld.
- [ ] Player defeat follows the game's normal recovery path.
- [ ] Mode disabled preserves the baseline encounter behavior.

## Calculation and state tests

When implementation starts, create meaningful automated tests for capture arithmetic, inventory transactions, and state transitions. Use explicit expected values from an independently checked FireRed/LeafGreen reference. A second copy of the implementation is not an independent oracle.

Cover full/low HP, each major status, catch-rate extremes, supported balls, guaranteed capture, and integer rounding boundaries. Fix RNG inputs to exercise failed and successful shake checks. Check probability bounds and verify that lower HP does not reduce the chance for otherwise equal inputs.

Test success/failure callbacks delivered more than once, cancellation, and interrupted cleanup. Inventory, storage, Pokedex updates, and rewards must remain consistent.

## Expansion and regression coverage

- Early game before and after the partner-play menu unlocks.
- Player team completely fainting during a wild encounter.
- Full party, available box space, full storage, and zero balls.
- Trainer battles and scripted/legendary encounters using their expected paths.
- Partner-only party, switched Pokemon, multiple participants, move learning, and evolution.
- Shiny Pokemon, forms, nature, individual stats, identity values, and moves surviving capture.
- Trapping moves and other effects persisting through failed throws.
- Save/reload after a catch or defeat, with no duplicate Pokemon or rewards.
- Catch Combo behavior matching the eventual documented decision.
- Keyboard/controller input and advertised languages.
- Eevee 1.0.0 and Pikachu 1.0.0 as separate results, followed by supported translation/file mods.

Public bug reports should include a minimal reproduction, version details, expected/actual behavior, and a sanitized log excerpt. Do not request game dumps, keys, or full saves as public attachments.
