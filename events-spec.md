This specification aims to describe the behaviour of the PocketMine-MP event handling system.

# Basics
In Minecraft gameplay it's desirable to be able to react to things taking place (events). PocketMine-MP has a built-in events system which allows third party code to react to, modify the outcome of, and prevent the result of events.

To do this, the following procedure is followed:
1. Something registers a handler for a given event.
2. Just before the event takes place, the handler is called and passed an object containing information about the event. This allows handlers to react to, modify (and in some cases prevent) an event from taking place.
3. The event takes place (or does not take place if cancelled) as defined by the object which contains the event information.

## Limitations
- It is currently not possible to have a handler execute after an event takes place. All event handlers are currently executed **before** the event takes place. This is a common pitfall of PocketMine-MP plugin developers - when an event handler is executed, the actual event has not yet taken place.

# Event handlers
As of 3.3.0, PocketMine-MP implements an architecture whereby a class implementing the `pocketmine\event\Listener` interface can have matching methods installed as handlers using reflection and annotations.

It is currently difficult and not worthwhile to install arbitrary callback handlers for events without a `Listener` object due to internal overcomplexity within PocketMine-MP. Because of this, this document will focus on event handlers defined by `Listener`s, and will not cover alternative ways of registering handlers.

## Definitions
- Listener: An object implementing the `pocketmine\event\Listener` interface, whose member functions will be treated as candidate handler functions.
- Handler: A function called before an event happens. This function will be given an object containing information about the event, allowing the handler to modify the outcome or (in some cases) prevent it from taking place entirely.

## The `pocketmine\event\Listener` interface
This interface is implemented by classes which want to have matching methods within themselves (and their child classes) to be treated as event handlers on registration. Every method in a `Listener`-implementing class is checked as a candidate event handler.

### Handler function names
Handler function names are **completely ignored**. Therefore, it doesn't matter whether your handler is called `onPlayerJoin()` or `iEatEnglishForBreakfast()`; as long as the function meets the required criteria described below, it will be registered.

### Handler registration criteria
To be considered as an event handler candidate, a `Listener` method MUST meet the below criteria:
- MUST accept exactly ONE parameter
- This one parameter MUST be a class extending `pocketmine\event\Event` which is either non-abstract or declares the `@allowHandle` in the class doc comment.
- MUST be public
- MUST NOT be static
- MUST be declared by a class implementing `Listener` or extending a class which implements `Listener` (i.e. a method in a non-`Listener`-implementing base class which meets the above criteria MUST NOT be considered a handler)
- MUST NOT declare the `@notHandler` annotation (will be ignored if it does)

The following are not mandatory but recommended:
- SHOULD NOT return a value (recommended to have a `void` return type (this may become a requirement in future))
- SHOULD NOT handle an event marked as `@deprecated`

### Annotations controlling a handler function's behaviour
Event handlers within a `Listener` can have their behaviour altered using doc-comment tags (otherwise known as "annotations") as per the PhpDoc standard.
The following annotations are respected for candidate handlers:
- `@notHandler`: Marks a function as explicitly NOT being an event handler, even if it meets all other criteria.
- `@ignoreCancelled`: This handler WILL NOT receive events which are cancelled before reaching it.
- `@priority`: Allows controlling when in the event calling sequence this handler will be executed. This allows competing handlers of the same event to cooperate to some extent. See the section below on event priority. Example: `@priority NORMAL`
- `@softDepend`: Skips registering the handler if the event class it wants to handle does not exist. This can be used to soft-depend on classes provided by other plugins. Example: `@softDepend SimpleAuth`

## Event handler priority
Event "priority" is used to control the order in which multiple handlers of the same event will execute. This is mainly used for plugin interoperability.
The `pocketmine\event\EventPriority` class declares a series of hardcoded priorities which handlers can use.

### Note
Priority is a misleading name, because higher "priority" means that the handler should execute _later_ in the sequence and not earlier. The reasoning for this is that higher order handlers "get the last word" over lower order handlers. This naming may be changed in future PocketMine-MP versions.

### List of priorities
At the time of writing, the following priorities exist:
- `LOWEST`: These handlers will be executed first. This should be used when the original event state is needed (before later handlers modify it) or when the handler is making a low-priority modification (to allow other handlers to override it).
- `LOW`
- `NORMAL`: If a priority is not specified, handlers will be registered here by default.
- `HIGH`
- `HIGHEST`: Handlers registered at this priority get the final say in the event's outcome.
- `MONITOR`: These handlers will execute last. **No modification to the event's outcome should be made at this priority, including cancellation**. This should be used to only for monitoring the outcome of an event.

### Caveats
- When multiple handlers are registered to the same priority, the order of execution is undefined.
