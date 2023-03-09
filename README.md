# *Eternal Card Game* Formats
A collection of settings files detailing *Eternal Card Game* official and community format rules, to be used by various community projects (ex. a deck format-legality checker).

*This is in no way affiliated or endorsed by Dire Wolf Digital. This is a project for and by the community.*

Implementation of these format rules is up to the project developers, though some code examples have been provided. Rules will need to be checked against data of Eternal's cards and their traits - this is not provided in this repo. *EternalWarcry* [provides current JSON data](https://eternalwarcry.com/cards/download) that can be used in your project.

# Settings files overview

The settings files are written in YAML (`.yml` files) for readabile syntax and simplicity. An example template file has been provided, with comments and an empty ruleset.

Before configuring the rules, a few fields provide meta info about the format: 

```yml
name: Some Format
description: Info about the format.
manager: What this format is for (ex. tournament series) / who maintains its rules.
```

Format rules are defined by a combination of fields to explicity include or exclude based on their traits (set, card type, rarity, specific names), as well as overrides to those rules:

```yml
rules:
  includeOnly:
    setTypes: []
    sets: [] 
    cardTypes: [] 
    rarities: [] 
    cards: [] 
  excludeAll:
    setTypes: []
    sets: [] 
    cardTypes: [] 
    rarities: [] 
    cards: []
  overrides:
    allowed: []
    banned: []
```

## Trait Fields
Include and exclude rules use the same trait sub-fields. Note that a trait field should **not** be both included and excluded in a ruleset - they define the same thing, but just approach it from different ends (ex. You could either list which rarities are allowed, or which ones are banned). Use whichever is the most concise or easiest to maintain.

Unused fields may use empty brackets or be removed entirely from your format rules. Project developers - be sure to account for sub-fields that don't exist!

### `setTypes`

You may broadly include or exclude multiple sets based on their type - `Large` for all primary pack-based sets, or `Small` for all campaigns and bundle sets.

Ex. Only use large sets in your format:

```yml
rules:
  includeOnly:
    setTypes:
      - Large
```

Note for project developers - you may need to either maintain an explicit list of sets and their types, or infer it based on set numbers (provided in the `SetNumber` field for each card in the *EternalWarcry* data). So far, DWD has conistently patterned set numbers for large sets with 1-2 digits (ex.`13` for *Unleashed*) and small sets with 4 digits (ex.`1135` for *Enter the Arcanum*).

### `sets`

List specific sets by name. 

Ex. Expedition sets at *Behemoths of Thera's* release:

```yml
rules:
  includeOnly:
    sets:
      - Unleashed
      - Enter the Arcanum
      - Behemoths of Thera
```

*Note that reprints may not be accounted for in your card data. You may need to add them in a `cards` field.*

### `cardTypes`

Card types to include or exclude. Current types are:

- Power
- Unit
- Spell
- Fast Spell
- Weapon
- Relic Weapon
- Relic
- Curse
- Cursed Relic
- Site

Ex. Only use Power and Units:

```yml
rules:
  includeOnly:
    cardTypes:
      - Power
      - Unit
```

### `rarities` 

Rarities to include or exclude. Current rarities are:

- Common
- Uncommon
- Rare
- Legendary
- Promo

Ex. Only use Common and Uncommon cards:

```yml
rules:
  includeOnly:
    rarities:
      - Common
      - Uncommon
```

Note that 'None' is also a rarity, but it should be avoided. It is mostly used by non-collectible 'token' cards that are created by game effects, which can't be included in decks. *EternalWarcry's* data lists these with a 'None' rarity and also flags them with a `DeckBuildable: false` field (developers may want to watch for `DeckBuildable: false` cards in case a user manually adds token cards into decklist text.)

The main exception to this rule are the basic Sigils - these have no rarity but are usable in decks. Rather than needing to explicitly include Sigils in all formats, developers should include them by default in their format processing (though you may still let format rules explicitly ban them).

### `cards`

List specific cards by name. 

Ex. Banning the cycle of original Scions:

```yml
rules:
  excludeAll:
    cards:
      - Kaleb, Uncrowned Prince
      - Talir, Who Sees Beyond
      - Rolant, the Iron Fist
      - Eilyn, Queen of the Wilds
      - Vara, Fate-Touched
```

## Overrides
Specific allowed or banned cards may be listed in the `overrides` sub-fields. These are a way to provide exceptions to the broader include/exclude rules.

Ex. A format of only Common cards, where Symbols are banned but the Rare Sketches are allowed.

```yml
rules:
  includeOnly:
    rarities:
      - Common
  overrides:
    allowed:
    banned:
      - Fire Sketch
      - Time Sketch
      - Justice Sketch
      - Primal Sketch
      - Shadow Sketch
    banned:
      - Fire Symbol
      - Time Symbol
      - Justice Symbol
      - Primal Symbol
      - Shadow Symbol
```