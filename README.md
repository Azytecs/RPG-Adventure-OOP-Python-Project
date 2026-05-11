# RPG Adventure — OOP Python Project

A text-based RPG game built in Python as an OOP coursework project. The player creates a character, explores a world, battles enemies, crafts items, and manages a save file — all driven through terminal input.

---

## How to Run

```bash
python import_random.py
```

To run the unit tests:

```bash
python test_rpg.py
```

---

## Project Structure

```
Project, OOP python/
├── import_random.py   # Main game logic (all classes + game loop)
└── test_rpg.py        # Unit tests for core mechanics
```

A `savefile.json` is created automatically in the same directory when you save.

---

## OOP Concepts Used

The project demonstrates the following OOP principles:

- **Inheritance** — `Goblin` and `Orc` both extend the base `Character` class
- **Method Overriding** — each enemy has its own `attack()`, `take_damage()`, and `take_turn()` logic
- **Encapsulation** — player stats and state are managed within the `Character` object
- **Polymorphism** — enemies are handled through a shared interface in the combat loop despite behaving differently

---

## Gameplay Overview

On each turn inside the game loop, the player can choose from four actions:

### ⚔️ Raid
Triggers a random combat encounter — 50% chance of a Goblin, 50% chance of an Orc. During combat the player can attack, defend, equip armour, or use a consumable. Enemies take turns after the player and behave differently depending on their type and current HP.

### 🌿 Explore
The player searches the area and has a chance to find one of five crafting materials: Iron Ore, Leather, Wood, a Vial, or Herbs. There is also a ~20% chance of finding nothing at all.

> **⚠️ Limitation:** Explored materials are added to the inventory but **cannot be used directly** — they only serve as inputs for the crafting system. There is no way to consume, drop, or sell them individually. If you don't have the right combination for a recipe, the items just sit in your inventory with no other purpose.

### 🔧 Craft
Opens the crafting menu where materials can be combined into usable items. Available recipes:

| Recipe | Ingredients | Result |
|---|---|---|
| Health Potion | Herbs + Vial | Restores 40 HP |
| Strong Health Potion | 2x Herbs + Vial | Restores 80 HP |
| Sharpening Oil | Herbs + Leather | Slight attack boost |
| Iron Sword | Iron Ore + Wood | Basic weapon |
| Leather Armour | 2x Leather | Light armour |

> **⚠️ Limitation:** Crafted items like Iron Sword and Leather Armour are added to the inventory but **have no in-game effect**. There is no mechanic to equip them or apply their stats — only the hardcoded "Armour Set" item (equipped during combat via option 3) actually does anything. Crafted weapons and armour are essentially dead weight.

### 🎒 Inventory
Displays all items currently held with their name, description, and category. Items cannot be used, dropped, or organised from this screen — it is view-only.

---

## Combat System

Turn-based combat with the following mechanics:

- **Player actions:** Attack, Defend (halves next hit taken), Equip Armour Set, Use Consumable
- **Critical hits:** 8% chance to double damage (both player and enemy)
- **Dodge:** 4% chance to take zero damage (both sides)
- **Armour Set:** Adds +3 to attack and reduces incoming damage by 3
- **Item drops:** After attacking or defending, there is a 12% chance of an Energy Fragment drop (+1–2 next attack), and a 4% chance of an Orcish Relic drop if the enemy is an Orc (+3–5 next attack and 10–30 HP restored)

### Enemy Behaviours

**Goblin** (80 HP, 6 ATK, 1 DEF)
- Flees when HP drops to 50 or below
- 10% chance per turn to use a special attack (4× damage)

**Orc** (130 HP, 10 ATK, 3 DEF)
- Enters rage mode when HP drops to 40 or below (uses special attack: 2× damage)

---

## Save System

Progress is saved to `savefile.json` using Python's `json` module. All player attributes are serialised and restored on load, including inventory, buff state, and cooldowns.

---

## Known Limitations

| Area | Issue |
|---|---|
| Crafted weapons/armour | Added to inventory but never applied — no equip mechanic |
| Exploration items | Can only be used in crafting; no other interactions |
| Sharpening Oil | Crafted but never consumed or applied anywhere in the code |
| Inventory | View-only — no use, drop, or equip options from this screen |
| Input validation | Most inputs only check for exact string matches; unexpected input prints "Invalid choice" and re-prompts |
| Save file | Saved to the current working directory; no file path selection |
| Single player | No support for multiple saves or characters |

---

## Tests

`test_rpg.py` covers:

- HP reduces correctly after `take_damage()`
- `is_alive()` returns correct result at HP 1 and HP 0
- Adding a single item to inventory
- Adding multiple items to inventory
- Goblin default stats (ATK 6, DEF 1)
- Orc default stats (ATK 10, DEF 3)
