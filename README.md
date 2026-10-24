# Pokemon Let's Go: Classic Wild Battles

An optional classic wild battle and capture mode for [Let's Go Recomp](https://github.com/raulzrrs/letsgo-recomp), using Pokemon FireRed and LeafGreen as the gameplay reference.

**Status: planning and source research. There is no playable mod, installer, or game build in this repository yet.**

## The goal

Keep Let's Go's world and presentation, and give wild encounters a classic battle loop:

- Weaken wild Pokemon with attacks and status conditions.
- Select and throw Poke Balls inside the battle scene.
- Calculate capture chances from species catch rate, current HP, status, and ball type.
- Continue the same battle after a failed capture, with the opponent taking its turn.
- Earn experience by defeating wild Pokemon.
- Choose the classic mode before starting a play session.

Playable Red and Green are a possible later addition. They are outside the first prototype.

## What already exists

The current upstream source goes further than its README's short wild battle description. It includes a CATCH command, HP/status modifiers, a return to battle after a failed throw, and an EXP hook. Its capture flow still switches to the native Let's Go capture scene.

This project will first reproduce that behavior, then investigate capture entirely within the battle scene. These are source observations, not results from running the game here. See the [pinned upstream research](docs/UPSTREAM_RESEARCH.md) for evidence and limitations.

The separate [Wild Battles mod for Switch 1.0.2](https://gamebanana.com/mods/729162) is a behavioral reference. Its author describes battle followed by the Let's Go capture mode, with no balls usable during battle and EXP still requiring a catch.

## Initial target

- Windows PC through Let's Go Recomp.
- Let's Go, Eevee! version 1.0.0 first.
- Let's Go, Pikachu! version 1.0.0 after the first implementation is verified.
- FireRed/LeafGreen capture rules as the intended reference, with explicit adaptations for Let's Go.

The Switch reference targets 1.0.2. That does not establish compatibility with the Recomp hooks. The existing Recomp mod manager supports file replacement; executable patches such as exefs and IPS are unsupported according to its documentation. Runtime source changes are the proposed integration route.

## Project documents

- [Gameplay specification](docs/GAMEPLAY.md)
- [Upstream research and sources](docs/UPSTREAM_RESEARCH.md)
- [Local runtime setup](docs/LOCAL_SETUP.md)
- [Baseline verification report](docs/BASELINE_REPORT.md)
- [Implementation roadmap](docs/ROADMAP.md)
- [Verification plan](docs/TEST_PLAN.md)
- [Contributing](CONTRIBUTING.md)

Start with milestone 0 in the roadmap. This repository currently contains the project specification and research. It does not vendor or fork the Recomp source. The code integration layout will be chosen after reproducing upstream behavior.

## Project boundaries

Game dumps, keys, saves, extracted assets, and generated game code belong in local development storage. They are excluded from this repository. Running the eventual implementation will require your own legally obtained game files and the upstream runtime's prerequisites.

This is an independent fan project, unaffiliated with Nintendo, The Pokemon Company, Game Freak, Creatures, or the authors of the referenced projects.

Original contributions in this repository use the [MIT license](LICENSE). That license grants no rights to the games or third-party work. Upstream code and third-party mods retain their own terms.
