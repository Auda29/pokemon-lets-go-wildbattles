# Gameplay specification

Status: intended behavior, with no implementation in this repository yet.

## Reference and scope

Pokemon FireRed and LeafGreen are the reference for classic catching and defeat-based experience. Kanto defines the setting; choosing FireRed/LeafGreen defines the intended mechanics. This does not imply converting Let's Go's entire combat system to Generation III.

Keep Let's Go's existing moves, damage rules, party, inventory, and presentation unless a change is needed for the wild encounter loop. Abilities, held items, breeding, EV conversion, and a full FireRed/LeafGreen remake are outside scope.

## Mode selection

Offer the classic mode as an explicit setting before a play session starts. Keep the existing upstream behavior available when the mode is disabled. The setting must not require a new game.

Defer changing the mode during an encounter. Compatibility with existing saves must be verified before advertising it.

## Wild encounter loop

1. Contact with an eligible wild Pokemon starts a battle against that encounter's Pokemon.
2. The player can fight, switch Pokemon, use supported items, attempt a capture, or run.
3. Choosing a supported ball from the battle bag commits a capture action.
4. Resolve and display the throw inside the battle scene.
5. On failure, consume the ball once, let the opponent take its turn, and return to action selection.
6. On success, transfer that encounter's Pokemon to the party or a box, update the Pokedex, and end the encounter once.
7. Defeating the opponent grants experience and ends the encounter without entering the Let's Go capture scene.

A committed throw takes the player's turn. Cancelling ball selection consumes neither a turn nor a ball. A rejected action, such as no storage space, must be detected before consuming the ball.

## Capture rules

Use FireRed/LeafGreen's Generation III capture calculation as the target. Resolve its exact integer arithmetic, rounding, and shake checks against a primary implementation before writing the algorithm. Do not label a generic HP-based formula as an exact reproduction.

Inputs must include the Pokemon's species catch rate, maximum HP, current HP, major status condition, and selected ball. The first prototype supports Poke Balls; Great, Ultra, and Master Balls follow before a general release. Other balls need explicit rules and tests.

Do not carry Let's Go's aim, ring size, throw quality, or berry bonuses into this calculation. A fainted Pokemon cannot be caught. Trainer-owned Pokemon cannot be caught. The Master Ball must guarantee capture for an otherwise valid action.

The first prototype may use a simple result message in the battle scene. A finished throw animation is a later presentation task; a switch to the Let's Go capture minigame does not meet the prototype's capture requirement.

## Encounter identity and battle state

The encountered, battled, and stored Pokemon must be the same individual. Preserve species, form, level, identity values, nature, shiny state, individual stats, and applicable moves. Determine the exact Let's Go fields through source research and runtime evidence.

A failed throw must preserve the full ongoing battle state, including both sides' HP and status, move PP, active Pokemon, stat stages, volatile effects, and turn counters. Reconstructing a fresh battle with only the opponent's HP restored is insufficient.

Prevent duplicate reward delivery, duplicate storage, or the defeated/caught encounter remaining interactable in the overworld.

## Experience

Use defeat-based EXP and participant sharing from FireRed/LeafGreen as the intended baseline. The classic mode grants no capture EXP or throw-quality EXP bonuses. Preserve the game's level-up, move-learning, and evolution handling.

The exact EXP calculation and its adaptation to Let's Go's party-wide distribution require investigation. Do not introduce held-item mechanics solely to reproduce Generation III's Exp. Share. Record any distribution differences before implementing them.

Keep the first prototype focused on one participating Pokemon. Multiple participants and party distribution are a separate acceptance gate before a general release.

## Special encounters and remaining decisions

Initially change ordinary wild encounters only. Trainer battles, scripted encounters, legendary encounters, GO Park, and story sequences retain their upstream paths until individually verified.

Before a general release, decide and document:

- How successful captures, knockouts, fleeing, and blackouts affect Catch Combos and associated bonuses.
- Which classic escape calculation to use and how trapping effects interact with capture actions.
- Which special encounters can use the classic flow without breaking story progression.
- How to handle all Let's Go-specific forms and partner Pokemon in captured data and battle models.

Red and Green as playable characters remain a later, independent feature. Reusing their models would still require checking animations, clothing, partner interactions, and story scenes.
