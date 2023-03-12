# Hello World with Ambient
### [Page0](index.md) [Page1](page1_hello.md) **([Page2](page2_entities_components))** [Page3]() [Page4]() [Page5]() 
## Entities and Components
Ambient is an ECS game engine.  ECS stands for "Entity Component System" -- 3 different but related pieces that make up the whole. We've already gotten a glimpse of two out of three of these on the previous page.  
* **Entities are things** -- like cubes or spheres and even the camera which is inserted into the world to view those things.  But weirdly, Entities are _things in the abstract_ -- they don't possess size or shape or color or ability to move.  Entities in Ambient are represented by an number, an `EntityID`, and not much else.  _That's_ how abstract they are!
* **Components are attributes** -- like _color_ or _position_ or _health_ or _if gravity has any affect_ on a thing (an Entity). Ambient even includes this Component: `"The height at which the fog will fall off (i.e. stop being visible) for this sun."` Components are added to entities to make them less absract.  Components include associated data, but not much else.  Like Entities, they are a bit disappointing.  They don't actually DO anything.  Beyond their presence as a sort of tag -- perhaps a "fat tag" with associated data -- all of the actual _doing_ takes place elsewhere.  Can you guess where?
* **Systems do stuff** -- Systems are responsible for everything you see, and that you see happening in an ECS game.  We'll figure out how later, but you should know they play a HUGE role.  If the Entities and Components seem somewhat boring, it's because all the fun happens in a System or two (or a hundred.)

## Components we've seen
Remember adding
```Rust
    .with(color(), random::<Vec3>().extend(1.0))
```
That line creates a `color()` Component with random RGB data and the alpha channel set to 1.0. Then the `color()` Component is added to the entity we were making.

## Entities we've seen
Looking back at the code we've been modifying, there are two Enties which are clearly instantiated in the code.  Here' how we made a cube show up:
```Rust
Entity::new()
    .with_merge(make_transformable())
    .with_default(cube())
    .with(color(), random::<Vec3>().extend(1.0))
    .spawn();
```
"new()" creates an entity and returns it, so we can chain the next 4 lines.  Each of the lines that start with `.with` are adding Components to the new Entity, then .spawn() launches it into existence in the game world and returns the `EntityID`, which we didn't save.

The camera Entity is also instantiated, appended with the desired characteristics via Components, and then spawned.

But there are a few _other_ Entities in our code. They are used as temporary holders of Components -- a Component package.  They are created by any routine that begins with `make_...`, like `make_tranformable()`.  If you hover over `make_tranformable()` in your code, you should see its description:
![](..\assets\make_transformable.png)  
Note that `make_transformable()` returns an Entity (a package of Components.)  The .with_merge() gobbles up that Entity and incorporates all of its Components into itself, discarding the input Entity. (In Rust parlance, the Entity produced by `make_transformable()` is consumed.)

This finally answers the question we asked at the bottom of the prior page:  
> "I don't know why _quads_ and _cubes_ just show up, but _spheres_ need to be _made_."  

Turns out, _spheres_ are a bit more complicated than _quads_ and _cubes_. _Spheres_ need a collection of related Components in order to be visible. `make_sphere()` creates a new Entity with all of the necessary Components pre-assembled.  When you merge the new sphere into your new Entity, your new entity becomes a visible sphere.  (Well, it will when you `spawn()` it.)
