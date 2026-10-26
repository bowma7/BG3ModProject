# Bag of Holding QoL Emporium

A native **Baldur's Gate 3 Toolkit** mod that adds a Bag of Holding with automatic inventory organization, manual sort/retrieve controls, weight management, persistent read book tracking, and party wide quality of life actions.

The project is built with BG3's native **Osiris Story scripting** and **Stats** systems and does **not** require Script Extender.

### Project Goal

The goal of Bag of Holding QoL Emporium is to make inventory management in Baldur's Gate 3 faster and cleaner while remaining conservative around game systems the mod cannot safely infer.

The project also serves as an exploration of event driven game scripting, runtime state management, cache validation, classification systems, compatibility focused design, and iterative refactoring inside a large existing game framework.

## Project Status

**Beta / active development**

The core inventory architecture has been implemented and tested in the BG3 Toolkit. The next phase is final release polish and longer form testing in a normal campaign.

## Main Features

- Automatically sorts recognized inventory items into nested organizers
- Supports three automatic sorting modes:
  - **Full Sort** — sorts all recognized supported categories
  - **Safe Sort** — avoids categories with known nested-inventory compatibility concerns
  - **Disabled** — disables automatic sorting while keeping manual controls available
- Supports two automatic triggers:
  - Sort loose inventory when the Bag is opened
  - Sort only the item placed into the Bag root
- Manual **Sort** and **Retrieve** actions for individual categories or all categories
- Managed items become effectively weightless while stored in the Bag hierarchy
- Items regain normal weight when returned to loose character inventory
- Tracks read books by template so duplicate copies are recognized as already read
- Integrates with BG3's native Keychain and Alchemy Pouch behavior
- Supports multiple Bag of Holding instances without breaking sorting
- Includes party wide Longstrider, Enhanced Leap, Feather Fall, and buff cleanup actions

The Bag of Holding contains a nested organizer hierarchy:

```
Bag of Holding
├── Equipment
│   ├── Weapons
│   ├── Armor
│   └── Accessories
├── Consumables
│   ├── Potions
│   ├── Scrolls
│   ├── Arrows
│   └── Throwables
├── Alchemy
├── Books & Notes - Unread
├── Books & Notes - Read
├── Key Ring
├── Quest & Important Items
└── Miscellaneous
```

## Design Philosophy

The goal is to automate repetitive inventory work without guessing about items the player may want to keep accessible for quests, combat, dialogue, or other game systems.

Party wide buffs should give quality of life functions rather than direct power or gameplay changes. (Buffs dont persist permanently during comabt) Any quality of life could be accomplished with native systems during normal gameplay although in a more tedious way.

## Development Journey

The project began as a straightforward nested Bag of Holding with category specific sorting rules.

As testing expanded, several BG3 specific problems appeared:

- Runtime container IDs differ from template IDs
- Nested inventory events can propagate through multiple ownership levels
- Some categories overlap, such as spell scrolls also being books in Larian's tagging system
- Some native game actions do not discover deeply nested items
- Broad global inventory listeners can generate large amounts of unnecessary work

The project was then refactored around reusable classifiers, validated runtime caches, explicit sorting policies, scoped event handling, and generic retrieval logic.

The current rewrite has been tested for:

- Full, Safe, and Disabled automatic sorting
- Bag Open and Item Enter triggers
- Manual sorting while automatic sorting is disabled
- Multiple Bag instances
- Stale/misplaced organizer detection and recovery
- Stack movement
- Weightless transfers and restoration
- Keychain and Alchemy Pouch retrieval safety
- Persistent Read/Unread book tracking
- Retrieval by category and Retrieve All

## Repository Structure

```
GeneratedStats/
    Generated/readable BG3 Stats definitions used by the mod

    Files here are .txt file type.

Released/
    Current packaged version of Mod .pak for local installation.   

Story/
    Osiris Story source files for sorting, retrieval,
    runtime target management, weight handling, and party QoL.

    Files here are .txt file type.

ToolkitStats/
    Toolkit facing Stats data such as spell, status,
    object, and treasure table definitions

    Files here are `.stats` which is a Larian-specific, XML based format. The files
    can be viewed as XML in a text editor, but are primarily
    intended to be interpreted and edited through the BG3 Toolkit.
```

The repository is intended as a portfolio oriented source snapshot rather than a full copy of the BG3 Toolkit workspace.

## Mod .pak Installation

Put the .pak in your normal BG3 Mods directory

The normal Windows location is:
```
C:\Users\<YourUserName>\AppData\Local\Larian Studios\Baldur's Gate 3\Mods
```

or equivalently:
```
%LOCALAPPDATA%\Larian Studios\Baldur's Gate 3\Mods
```

Local .pak files placed there can be detected by the normal game/mod management setup.

Also see : https://baldursgate3.game/mods-how-to/
