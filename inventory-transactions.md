# Inventory Transactions

Inventory transactions are a system used to group changes to inventories, dropped items or other item stack locations. This system is used to provide a coherent view of activity in inventories caused by players, and to prevent cheating or creating/destroying items in multiplayer races.

## Validation
A standard inventory transaction is valid when the following conditions are met:
- The computed balance of the transaction is zero. This means:
  - Every input must be matched by an equal number of identical outputs.
  - No extra items can be created (outputs without a balancing input)
  - No items can be destroyed (inputs without a balancing output)
  - Item userdata may not change on any item (custom names, lore, etc)
- The transaction must have at least 1 action.
- All actions in the transaction must consider themselves valid. This includes checks such as validating that the input item stack for a slot change matches the existing stack in the inventory.

## Inventory actions
An inventory action describes a single change to an item stack involved in the transaction.
It has two primary components:
- Input (for validation): The item stack that should exist in the target location before the transaction takes place.
- Output (for execution): The item stack that will be set into the target location once the transaction is completed.

A transaction is composed of an **unordered set** of inventory actions. The system is **explicitly designed _not_ to care about ordering, so your plugin should also not depend on any ordering**.

### Inventory action types
There are several types of inventory actions which provide different methods of modifying the balance of a transaction.

#### Slot change
A standard change in an inventory or container, e.g. a chest. The target location is a specific slot number in the inventory.
To be valid, the input item for this action type must match the item in the inventory at the point of validation.

#### Drop item
An item stack dropped into the world as an entity. This has no validation criteria other than the usual transaction balancing requirements.

#### Creative inventory
An item stack being created or destroyed. This action type is only legal when the source player is in creative mode (and the item must exist in the creative inventory when creating items).
