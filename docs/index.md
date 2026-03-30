# Interior Scene DSL

An interior-design-aware scene description language for building indoor scenes through:

1. **object registration**
2. **object placement via groups**
3. **scene optimization**

This documentation is a practical guide to the language itself: what it can express, how to compose scenes, and how to use its grouping abstractions to build realistic layouts.

For the broader motivation and design principles behind the language, please refer to the paper.

---

## What this language enables

With this language, you can:

- register objects from natural-language descriptions,
- compose semantic arrangements such as seating areas, bed areas, dining layouts, and study corners,
- place objects relative to anchors or around other objects,
- arrange assets in grids and rectilinear layouts,
- assemble full rooms with wall-aware placement,
- add architectural elements such as windows and doors,
- run optimization to improve scene plausibility.

---

## Core workflow

Most scenes follow the same pattern:

1. create a scene,
2. register assets,
3. organize them with groups,
4. place groups into a room,
5. export the final result.

A typical scene looks like this:

```python
from scene import SceneProgRoom

scene = SceneProgRoom("simple_living_room")

with scene.RelativeGroup() as seating:
    sofa = scene.AddAsset("a modern 3-seat sofa")
    coffee_table = scene.AddAsset("a rectangular wooden coffee table")
    seating.set_anchor(sofa)
    seating.place_on_front(coffee_table)

with scene.RoomGroup() as room:
    room.place_on_back_wall_center(seating, facing="front")
    room.place_walls(
        floor_texture="light oak wood floor",
        ceiling_texture="smooth white ceiling",
        wall_texture="warm off-white painted wall",
    )

scene.export("simple_living_room.blend")
