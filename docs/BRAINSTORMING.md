# MoonBox

## Concept

MoonBox is a cozy yet intelligent shell framework for a TermuxLite-based environment.

It is meant to make that environment feel clearer, more guided, more memorable, and easier to navigate while remaining transparent, modular, and local-first.

MoonBox should feel like a friendly, well-structured terminal environment rather than a pile of disconnected tools.

## Status

Planning / Architecture Draft

## Why MoonBox Exists

Termux and terminal tooling are powerful, but they can also feel fragmented, intimidating, or hard to remember.

MoonBox exists to explore a better experience layer for that ecosystem:

- more welcoming for nontechnical and neurodivergent users,
- easier to learn and remember,
- more guided without becoming restrictive,
- more local-first and self-host-friendly,
- and more cohesive across setup, commands, diagnostics, packages, and optional AI workflows.

The goal is not to hide the terminal.

The goal is to make the terminal feel calmer, clearer, and more humane.

## Design Pillars

- Neurodivergent & NonTechnical-Friendly
- Self-Host/LocalHost-Friendly
- AI-Augmented
- Mnemonic-style Aliases

## Working Principles

- Local-first over cloud-first
- Transparency over magic
- Guidance over intimidation
- Modularity over bloat
- Friendly UX without becoming a toy
- Rootless-first assumptions
- Optional intelligence, not forced intelligence
- Good defaults with clear escape hatches for advanced users

## Product Shape

MoonBox should feel like a combination of:

- a TermuxLite-based environment,
- a Zsh-first shell framework,
- a Gum-powered interaction layer,
- a modular plugin/setup system,
- a mnemonic command and alias experience,
- and an optional entrypoint into AI-assisted and self-host-friendly workflows.

MoonBox is best understood as an orchestration and UX framework, not a replacement for every tool it uses.

## What MoonBox Is

MoonBox is:

- a cohesive shell experience,
- a framework for organizing terminal capabilities,
- a friendlier interaction layer for common tasks,
- a place for mnemonic aliases and memorable workflows,
- and a modular base for future extensions.

## What MoonBox Is Not

MoonBox is not:

- a full rewrite of TermuxLite,
- a full rewrite of Termux App Store,
- a full rewrite of TDOC,
- a replacement for Zsh itself,
- or an attempt to hide the system behind opaque abstractions.

It should improve experience and cohesion without erasing the underlying tools.

## Foundations

- https://github.com/zsh-users/zsh
- https://github.com/radiator13/TermuxLite
- https://github.com/charmbracelet/gum

## Optional Integrations / Future Adapters

- https://github.com/charmbracelet/crush
- https://github.com/charmbracelet/soft-serve

## Integrated / Adapted Tools

### Termux App Store
- https://github.com/djunekz/termux-app-store

Intended use in MoonBox:
- preserve the package-management purpose and core logic,
- wrap or supplement shell-facing interactive flows with Gum where appropriate,
- and improve consistency with the broader MoonBox command and UX style.

MoonBox should not assume it needs to replace the native TUI or CLI wholesale.

It should provide a calmer, more guided shell-facing layer where that improves usability.

### TDOC
- https://github.com/djunekz/tdoc

Intended use in MoonBox:
- preserve the diagnostic, repair, reporting, monitoring, and scanning purpose and core logic,
- wrap or supplement shell-facing interactive flows with Gum where appropriate,
- and make scans, explanations, reports, and next actions easier to follow inside MoonBox.

MoonBox should not assume it needs to replace TDOC’s native behavior wholesale.

It should focus on friendlier shell-facing guidance around existing capabilities.

## Plugin / Module Loader Inspiration

### Gaiety
- https://github.com/calafite/gaiety

Potential inspiration:
- manifest-driven modules,
- dependency-aware loading,
- public API declarations,
- structured reload behavior,
- and optional lazy or event-driven loading.

### Zero Terminal
- https://github.com/zerolinux-os/zero_terminal

Potential inspiration:
- deterministic plugin loading,
- doctor-style workflows,
- plugin isolation and safety-minded patterns,
- security scanning ideas,
- and backup / rollback thinking.

### zsh_unplugged
- https://github.com/mattmc3/zsh_unplugged

Potential inspiration:
- understandable plugin loading,
- lightweight and transparent shell customization,
- and avoiding unnecessary manager complexity.

## UX Direction

MoonBox should favor:

- readable prompts,
- guided choices,
- clear confirmations,
- consistent command naming,
- memorable aliases,
- low-stress wording,
- and visible system state.

Common interactive flows should feel like:
- choose,
- inspect,
- confirm,
- run,
- review.

Gum is central to that experience where menus, filtering, confirmation, paging, spinners, styled output, and simple shell-native interaction patterns improve usability.

## Alias Direction

Mnemonic-style aliases should be treated as a major product feature, not a side convenience.

They should be:

- memorable,
- readable,
- guessable,
- teachable,
- and consistent.

Aliases should help users build confidence, not dependence on mystery shortcuts.

## Command Model Direction

MoonBox should use one primary command namespace rather than several unrelated top-level commands.

Primary command:
- `mb`

Supported long form:
- `moonbox`

The command model should favor:

- one memorable entrypoint,
- noun-based command families,
- verb-based actions inside those families,
- friendly interactive behavior when no action is provided,
- and short mnemonic aliases layered on top of the full command structure.

### Core Pattern

Primary pattern:
- `mb <family> <action> [args]`

Examples:
- `mb doctor scan`
- `mb doctor fix`
- `mb pkg search <query>`
- `mb pkg install <package>`
- `mb mod list`
- `mb mod enable <name>`

This structure should keep MoonBox understandable, teachable, and scalable.

### Entry Pattern

`mb` itself should be the main home entrypoint.

Examples:
- `mb` -> MoonBox home / main menu
- `mb help` -> help and command overview
- `mb where` -> environment or framework location/status overview

### Interactive Pattern

When the user stops at the family level, MoonBox should prefer a guided Gum-based experience.

Examples:
- `mb doctor` -> doctor menu
- `mb pkg` -> package browser or package actions menu
- `mb mod` -> module manager menu

This supports the intended flow:
- choose,
- inspect,
- confirm,
- run,
- review.

## Core v1 Families

These are the first-class command families for an early MoonBox MVP.

### Setup
Bootstrap, onboarding, and initial environment preparation.

Examples:
- `mb setup`
- `mb setup check`
- `mb setup init`
- `mb setup reset`

### Doctor
Diagnostics, explanation, repair, reporting, and health visibility.

Examples:
- `mb doctor`
- `mb doctor scan`
- `mb doctor explain`
- `mb doctor fix`
- `mb doctor report`
- `mb doctor live`

### Pkg
Package browsing, inspection, install, update, and removal flows built around a MoonBox-friendly layer over Termux App Store.

Examples:
- `mb pkg`
- `mb pkg browse`
- `mb pkg search <query>`
- `mb pkg show <package>`
- `mb pkg install <package>`
- `mb pkg upgrade`
- `mb pkg remove <package>`

### Mod
Module and plugin management.

Examples:
- `mb mod`
- `mb mod list`
- `mb mod info <name>`
- `mb mod enable <name>`
- `mb mod disable <name>`
- `mb mod reload`
- `mb mod new <name>`

## Later / Optional Families

These families fit MoonBox, but do not need to be core in the first MVP.

### Shell
Framework-level shell and session actions.

Examples:
- `mb shell status`
- `mb shell reload`
- `mb shell theme`

Boundary:
- `shell` is for MoonBox framework or session behavior,
- `doctor` is for diagnostics, repair, monitoring, scanning, and reporting.

### AI
Optional AI-assisted workflows.

Examples:
- `mb ai`
- `mb ai chat`
- `mb ai code`
- `mb ai explain`
- `mb ai fix`

These capabilities are optional extensions, not part of MoonBox’s minimum identity.

### Host
Localhost and self-host-related workflows.

Examples:
- `mb host`
- `mb host git`
- `mb host serve`
- `mb host status`

These capabilities are optional extensions, not part of MoonBox’s minimum identity.

## Command Naming Rules

MoonBox commands should be:

- readable before clever,
- consistent before short,
- guessable before dense,
- and inspectable before automated.

A user should be able to look at a command and make a reasonable guess about what it does.

## Alias Strategy

MoonBox should always document and support the full command form first.

Aliases should be an additional usability layer, not the only interface.

Recommended alias layers:

- full form for clarity, example: `mb doctor scan`
- short mnemonic forms for speed, example: `mb ds`
- friendly helper aliases for common tasks, example: `heal`, `shop`, `mods`

Alias rules should aim for:
- easy recall,
- family consistency,
- minimal ambiguity,
- discoverability through help output,
- avoidance of risky collisions with common shell commands,
- and a curated default set rather than uncontrolled alias sprawl.

Potential examples:
- `mb ds` -> `mb doctor scan`
- `mb df` -> `mb doctor fix`
- `mb ps` -> `mb pkg search`
- `mb pi` -> `mb pkg install`
- `mb ml` -> `mb mod list`
- `mb me` -> `mb mod enable`

Friendly examples:
- `heal` -> `mb doctor fix`
- `shop` -> `mb pkg browse`
- `mods` -> `mb mod list`

## Alias Governance

MoonBox should treat aliases as a governed system, not a free-form shortcut dump.

Rules:
- full commands remain the primary documented interface,
- first-party aliases should be curated and limited,
- aliases must be declared visibly in help output,
- aliases must be checked for collisions before activation,
- modules must not inject aliases silently,
- and destructive actions should not rely on cryptic alias naming alone.

MoonBox should reserve consistent short patterns for official first-party aliases where possible.

## Relationship To Upstream Tools

MoonBox should not erase upstream tools.

Instead, the command model should provide a friendlier shell-facing layer around them.

Examples:
- doctor-related commands can wrap, guide, or simplify TDOC-oriented workflows,
- pkg-related commands can wrap, guide, or simplify Termux App Store workflows,
- module-related commands can follow ideas inspired by Gaiety, Zero Terminal, and zsh_unplugged,
- and Gum should shape interaction where menu selection, filtering, confirmation, paging, spinners, or styled status output improves usability.

Upstream logic should remain visible and inspectable wherever possible.

## Wrapper Policy

MoonBox should prefer a hybrid interaction model:
- wrapper-first for common guided flows,
- native upstream invocation preserved for advanced or exact behavior,
- clear handoff points when users want the original tool directly,
- and no attempt to fake full feature parity where MoonBox does not yet provide it.

If a MoonBox wrapper cannot faithfully support an advanced flow, it should hand off to upstream behavior rather than obscure it.

## Modularity Direction

MoonBox should grow through modules rather than one giant monolith.

Possible module categories:
- shell UX,
- setup/bootstrap,
- doctor/health,
- package browsing/install helpers,
- Git/project helpers,
- AI workspace helpers,
- self-host/local server helpers,
- plugin/module management.

Modules should ideally be:
- understandable,
- optional,
- removable,
- safe to inspect,
- and implemented as a hybrid model of shell logic plus lightweight metadata.

## Module Format

A MoonBox module should be a hybrid unit made of:
- shell implementation,
- lightweight metadata,
- optional command exposure,
- optional aliases,
- and optional hooks for load or enable behavior.

Metadata should define things like:
- module id,
- display name,
- summary,
- dependencies,
- commands exposed,
- aliases exposed,
- enable state,
- and load behavior.

Shell logic should contain the real implementation.

Metadata should describe how MoonBox loads, displays, enables, disables, and explains the module.

## Family-To-Module Relationship

Command families and modules should not be treated as the same thing.

Families are the stable user-facing command structure.

Modules are the implementation and extensibility structure behind that command surface.

This means:
- core families may be backed by first-party core modules or internal module-like components,
- not every module needs its own top-level family,
- and MoonBox can grow internally without forcing the CLI to become noisy or unstable.

## Safety Direction

MoonBox should assume users want reassurance before trust.

This suggests:
- visible actions before execution,
- confirm-before-destructive behavior,
- readable status outputs,
- simple rollback or recovery thinking where possible,
- and minimal hidden automation.

A calm framework should not surprise the user in dangerous ways.

## AI Direction

AI should be present as an optional augmentation layer, not as the whole identity of the project.

MoonBox can support:
- AI-assisted shell workflows,
- terminal-friendly coding assistance,
- guided troubleshooting,
- and local or self-host-compatible workflows.

Crush is relevant here as inspiration or optional integration for terminal-native AI work.

These capabilities are optional extensions, not part of MoonBox’s minimum identity.

## Self-Host Direction

MoonBox should remain friendly to localhost and personal infrastructure.

That may include:
- local-only development flows,
- self-host-friendly Git workflows,
- optional Soft Serve integration,
- and patterns that do not assume cloud-first services.

This should feel empowering, not enterprise-heavy.

These capabilities are optional extensions, not part of MoonBox’s minimum identity.

## Scope Decisions

Core for v1:
- setup,
- doctor,
- pkg,
- mod,
- shell UX foundations,
- the command model,
- the alias model,
- and the minimal module contract.

Optional for later:
- AI family,
- Host family,
- deeper self-host integrations,
- and richer external adapters.

## Presentation Direction

MoonBox should feel cozy in tone, not childish in presentation.

That means:
- calm wording over cutesy wording,
- reassuring interaction over decorative flourish,
- warmth without vagueness,
- and personality without losing clarity.

Cozy should describe the emotional tone of the UX, not a decorative visual theme layered on top of the terminal.

## Non-Goals

At least for the early stages, MoonBox should avoid trying to become:

- a complete Linux distribution,
- a full package ecosystem replacement,
- a highly abstract “one command does everything” black box,
- a giant UI shell that fights the terminal,
- or a project that rewrites stable upstream logic unnecessarily.

## Early MVP Ideas

### MVP 1 — Identity + Core Contract
- establish the `mb` command model and family structure,
- define voice and tone,
- create baseline Gum-driven interaction patterns,
- define alias families,
- and define the minimal module contract.

### MVP 2 — Bootstrap / Setup
- onboarding flow,
- basic environment checks,
- profile or mode selection,
- and a welcoming first-run experience.

### MVP 3 — Doctor Layer
- MoonBox-friendly entrypoints around diagnostics,
- clearer explanations, reports, and guided next actions,
- and Gum-based interaction for shell-facing doctor flows.

### MVP 4 — Package UX Layer
- MoonBox-friendly entrypoints around package discovery and install flows,
- a more cohesive shell-facing interactive layer around Termux App Store workflows,
- and clearer package browsing and action paths.

### MVP 5 — Module UX Expansion
- richer module enable/disable/list/reload concepts,
- module inspection and explanation flows,
- and a structure that keeps future growth manageable.

## Resolved Direction

MoonBox’s current direction is:

- primary command: `mb`
- supported long form: `moonbox`
- core v1 families: `setup`, `doctor`, `pkg`, `mod`
- alias stance: curated, documented, mnemonic, optional
- wrapper stance: wrapper-first for guided flows, native upstream access preserved
- module format: hybrid, shell implementation plus lightweight metadata
- minimal module contract included in v1
- core scope: shell UX, setup, doctor, pkg, mod
- optional scope: AI, Host, deeper self-host features, and extra integrations
- presentation stance: cozy in tone, clear in behavior, never vague or overly cute

## Summary Direction

MoonBox should become a calm, memorable, modular shell framework for a TermuxLite-based environment.

Its differentiators are:
- cohesion,
- friendliness,
- mnemonic usability,
- Gum-centered interaction,
- modular structure,
- and support for local-first, AI-augmented, self-host-capable workflows.
