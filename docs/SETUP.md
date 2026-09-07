# SETUP.md

## Purpose

This document is the implementation companion to `BRAINSTORMING.md`.

`BRAINSTORMING.md` defines what MoonBox is, why it exists, and the direction it has
committed to. `SETUP.md` defines how that direction gets built: repository layout,
install flow, module contract, command dispatch, and the concrete conventions
this project follows.

This document exists to keep implementation decisions consistent as MoonBox is
built solo. It is not written for external contributors, since this project does
not accept outside contributions — it exists mainly for personal reference and
clarity. As MoonBox matures, a separate user-facing install guide may be split
out from this file.

## Status

Early Implementation Draft

## 1. Prerequisites

MoonBox targets a TermuxLite-based environment but should degrade gracefully on
standard Termux or compatible Linux shells where reasonable.

Required:

- Zsh (minimum version TBD, target parity with modern Zsh feature expectations)
- Git
- curl or wget
- Gum (charmbracelet/gum) binary available on PATH

Assumed environment defaults:

- Rootless-first
- No forced network dependency for core commands (setup, doctor, mod)
- Package-related commands (`pkg`) may depend on Termux App Store availability

## 2. Repository Layout

Proposed top-level structure:

```
moonbox/
├── bin/                  # mb entrypoint script
├── core/                 # dispatcher, loader, shared runtime logic
├── families/             # setup, doctor, pkg, mod (core v1 command families)
├── modules/               # hybrid module units (shell + metadata)
├── lib/                   # shared shell helpers (formatting, prompts, safety checks)
├── docs/
│   ├── BRAINSTORMING.md
│   └── SETUP.md
├── .github/
│   ├── workflows/         # CI definitions (md-readi-lint, future checks)
│   └── scripts/           # local-only tooling (gitignored)
├── .gitignore
└── README.md
```

This layout is a draft, not a contract. It should be revisited once the dispatcher
and module loader have working implementations.

## 3. Install Flow (Draft)

MoonBox should install without silently overwriting user shell configuration.

### 3.1 Guarded Shell Block

Instead of overwriting `.zshrc`, MoonBox should inject a guarded, clearly delimited
block:

```
# >>> MOONBOX START >>>
export MB_HOME="$HOME/.moonbox"
source "$MB_HOME/core/loader.zsh"
# <<< MOONBOX END >>>
```

The installer must detect whether this block already exists before appending it
again, to avoid duplicate sourcing.

### 3.2 Backup Before Touching Anything

Before any modification, the installer should create a timestamped backup:

```
~/.moonbox_backup/<timestamp>/
├── zshrc.bak
├── moonbox_home.bak/
└── restore.sh
```

`restore.sh` should fully reverse the install by restoring the backed-up `.zshrc`
and removing the guarded block.

### 3.3 Suggested Install Command Shape

```
git clone https://github.com/<org>/moonbox ~/.moonbox-src
bash ~/.moonbox-src/install.sh --yes
exec zsh
```

Flags to support early:

- `--yes` — skip confirmation prompts
- `MB_SAFE_MODE=1` — start with no modules loaded
- `MB_LOG_LEVEL=0|1|2|3` — debug/info/warn/error

## 4. Minimal Module Contract (v1)

A MoonBox module is a hybrid unit: shell implementation plus lightweight metadata.

### 4.1 Directory Shape

```
modules/<name>/
├── module.mb          # metadata (id, name, summary, dependencies, commands, aliases)
├── init.zsh           # defines module_init()
└── commands.zsh       # defines module_register_commands()
```

### 4.2 Metadata Fields (Draft)

- `id` — unique machine-readable identifier
- `name` — display name
- `summary` — one-line description
- `dependencies` — list of required modules or binaries
- `commands` — list of commands this module exposes
- `aliases` — list of aliases this module registers
- `enabled_by_default` — boolean

### 4.3 Loader Responsibilities

- Parse `module.mb` before sourcing shell files
- Resolve dependencies before enabling a module
- Call `module_init()` then `module_register_commands()`
- Unset both functions after invocation to avoid name leakage between modules,
  following the isolation pattern used by comparable Zsh frameworks

## 5. Command Dispatcher (`mb`)

### 5.1 Core Pattern

```
mb <family> <action> [args]
```

### 5.2 Entry Behavior

- `mb` alone -> MoonBox home / main menu (Gum-driven)
- `mb help` -> command and alias overview
- `mb <family>` alone -> guided Gum menu for that family
- `mb <family> <action>` -> direct execution, no menu

### 5.3 Resolution Order

1. Match exact family + action from core families
2. If family exists but action is missing, launch that family's Gum menu
3. If no family matches, attempt alias resolution
4. If nothing resolves, show help with suggested closest matches

## 6. Core v1 Families — Implementation Notes

### 6.1 Setup

- `mb setup check` — validate Zsh version, Gum presence, MB_HOME integrity
- `mb setup init` — first-run bootstrap and guided onboarding
- `mb setup reset` — restore to a clean state, confirm before destructive action

### 6.2 Doctor

- `mb doctor scan` — run diagnostics, wrapping TDOC-oriented logic where applicable
- `mb doctor explain` — human-readable explanation of findings
- `mb doctor fix` — guided repair flow, confirm before destructive changes
- `mb doctor report` — generate a readable summary
- `mb doctor live` — continuous/live health view

Suggested output style, PASS/WARN/FAIL summary:

```
PASS: 24   WARN: 1   FAIL: 0
```

### 6.3 Pkg

- `mb pkg browse` / `search` / `show` / `install` / `upgrade` / `remove`
- These should wrap Termux App Store flows, not replace them
- Handoff rule: if a flow requires advanced or exact upstream behavior MoonBox does
  not yet guide well, defer directly to the native Termux App Store command

### 6.4 Mod

- `mb mod list` — show all modules with enabled/disabled state
- `mb mod info <name>` — show metadata and summary
- `mb mod enable <name>` / `disable <name>` — toggle state, update tracking
- `mb mod reload` — re-run the loader
- `mb mod new <name>` — scaffold a new module directory from a template

## 7. Alias Registration & Governance (Implementation)

- Aliases should be declared in a central registry file for first-party aliases,
  separate from per-module alias declarations
- At load time, the loader must check for collisions between:
  - first-party aliases
  - module-declared aliases
  - common shell builtins/commands
- On collision, the loader should warn and skip registering the colliding alias
  rather than silently overriding it
- `mb help` output must show, per command: full form, alias (if any), and family

## 8. Safety & Rollback

- Destructive actions (`doctor fix`, `pkg remove`, `mod disable`, `setup reset`)
  must require confirmation unless `--yes` is passed
- Every install/modify operation affecting shell configuration must be reversible
  via a generated `restore.sh`
- Uninstall must remove the guarded block, delete `MB_HOME`, and remove any
  symlinked `mb` binary

## 9. Gum Interaction Patterns

Standard flow for family-level menus:

1. Choose — Gum-driven list/filter of available actions
2. Inspect — show details of the selected action before running
3. Confirm — explicit confirmation step for anything non-trivial
4. Run — execute with visible status (spinner/progress where relevant)
5. Review — show a clear result summary

Styling conventions (draft):

- Consistent color usage for success/warning/error states
- Spinners only for operations expected to take longer than roughly one second
- Avoid decorative Gum usage that does not serve the choose/inspect/confirm/run/review flow

## 10. Configuration

User-overridable settings should live in `~/.moonboxrc`:

```
# ~/.moonboxrc

# Log level: 0=DEBUG 1=INFO 2=WARN 3=ERROR (default: 1)
MB_LOG_LEVEL=1

# Block unsafe modules from loading (default: warn only)
MB_STRICT_SAFETY=0

# Local-only startup timing log, no network calls (default: off)
MB_TELEMETRY=0
```

## 11. Testing & Verification

Manual smoke test checklist for MVP 1:

- [ ] `mb` opens the home menu without error
- [ ] `mb help` lists all core families, actions, and aliases
- [ ] `mb setup check` correctly detects missing prerequisites
- [ ] `mb doctor scan` runs without requiring network access
- [ ] `mb mod list` shows at least the built-in/core modules
- [ ] Install script produces a valid backup and a working `restore.sh`
- [ ] Uninstall fully reverses the install with no leftover guarded block

## 12. Open Implementation Questions

- What format should `module.mb` metadata use: YAML, TOML, or plain shell key=value?
- Should the alias registry be a single file or distributed with fallback
  collision-checking across modules?
- Should `MB_HOME` default to `~/.moonbox`, or follow a TermuxLite-specific
  convention instead?
- Should `doctor` wrap TDOC directly as a dependency, or reimplement a thin
  MoonBox-native diagnostic layer that calls TDOC where available?
- What is the minimum Zsh version MoonBox should officially support?

These questions should be resolved incrementally as MVP 1 and MVP 2 are implemented,
and this document should be updated as answers are settled.
