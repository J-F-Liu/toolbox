# Bevy Engine

[Bevy](https://bevyengine.org/)
[First Read](https://github.com/jamadazi/bevy-cheatsheet/blob/master/bevy-cheatsheet.md)
[learn-bevy](https://dev.to/ethanyidong/the-bevy-engine-1-4k4k)

## Tutorials

https://mbuffett.com/posts/bevy-snake-tutorial/
https://whoisryosuke.com/blog/2023/making-galaga-in-rust-with-bevy-part-1/
[A Short Introduction to Rust and the Bevy Game Engine](https://wiki.wptcsu.com/en/cpt/howto/game-dev/rust-bevy-workshop)

## ECS

All app logic in Bevy uses the Entity Component System paradigm, which is often shortened to ECS.
Entities are unique "things" that are assigned groups of Components, which are then processed using Systems.

```rust
struct Entity(u64);
```

App contains the ECS World, Resources, Schedule, and Executor.

```rust
pub struct App {
    pub world: World,
    pub resources: Resources,
    pub runner: Box<dyn Fn(App)>,
    pub schedule: Schedule,
    pub executor: ParallelExecutor,
    pub startup_schedule: Schedule,
    pub startup_executor: ParallelExecutor,
}
```

`AppBuilder` is a builder used to add stags, systems, resources, events and plugins to app.

```rust
App::build()
    .add_default_plugins()
    .add_resource(resource)
    .init_resource<R>() // resource = R::from_resources(&self.app.resources)
    .add_startup_stage(stage_name)
    .add_stage(stage_name)
    .add_startup_system(system) // added to stage::STARTUP
    .add_startup_system_to_stage(system, stage_name)
    .add_system(system) // added to stage::UPDATE
    .add_systems(systems) // added to stage::UPDATE
    .add_system_to_stage(system, stage_name)
    .init_system(build) // create a system with build function and add to stage::UPDATE
    .init_system_to_stage(build, stage_name)
    .add_event::<T>()
    .add_plugin(plugin)
    .run();
```

Default stages:

- startup_stage::STARTUP
- startup_stage::POST_STARTUP
- stage::FIRST
- stage::EVENT_UPDATE
- stage::PRE_UPDATE
- stage::UPDATE
- stage::POST_UPDATE
- stage::LAST

## System

```rust
pub trait System: Send + Sync {
    fn name(&self) -> Cow<'static, str>;
    fn id(&self) -> SystemId;
    fn update_archetype_access(&mut self, world: &World);
    fn archetype_access(&self) -> &ArchetypeAccess;
    fn resource_access(&self) -> &TypeAccess;
    fn thread_local_execution(&self) -> ThreadLocalExecution;
    fn run(&mut self, world: &World, resources: &Resources);
    fn run_thread_local(&mut self, world: &mut World, resources: &mut Resources);

    fn initialize(&mut self, _resources: &mut Resources) { ... }
}

pub trait IntoForEachSystem<CommandBuffer, R, C> {
    fn system(self) -> Box<dyn System>;
}

impl<Func, A> IntoForEachSystem<(Commands,), (), (A,)> for Func
where
    Func: FnMut(Commands, A) + FnMut(Commands, <<A as HecsQuery>::Fetch as Fetch>::Item) + Send + Sync + 'static,
    A: HecsQuery,

impl<Func, A> IntoForEachSystem<(), (), (A,)> for Func
where
    Func: FnMut(A) + FnMut(<<A as HecsQuery>::Fetch as Fetch>::Item) + Send + Sync + 'static,
    A: HecsQuery,
...
```

`Commands` is a builder used to queue commands to mutate world and resources.

- `commands.spawn(components)` Create an entity with certain components.
- `commands.with(component)` Add a component to previously created entity.
- `commands.with_bundle(components)` Add multiple components to previously created entity.
- `commands.insert(entity, components)` Add multiple components to specified entity.
- `commands.insert_one(entity, component)` Add one component to specified entity.
- `commands.remove_one::<T>(entity)` Remove `T` component from specified entity.
- `commands.write_world(world_writer)` Mutate world with custom world writer.

Systems run in parallel by default when they have no shared dependencies.
Startup systems are just like normal systems, but they run exactly once, before all other systems, right when our app starts.

## Plugin

Bevy's Default Plugins adds the features most people expect from an engine, such as a 2D / 3D renderer, asset loading, a UI system, windows, and input.

- `WinitPlugin` which uses the winit library to create a window using your OS's native window api.
- `WindowPlugin` which defines the window interface (but doesn't actually know how to make windows).
- adds an "event loop" to our application. Our App's ECS Schedule now runs in a loop once per "frame".

Default Plugins:

- LogPlugin: set log filter and level
- CorePlugin: setup task pools, register types, `Res<FrameCount>`
- TimePlugin: update time, `Res<Time>`,`Res<TimeUpdateStrategy>`,`Res<FixedTimesteps>`
- TransformPlugin
- HierarchyPlugin
- DiagnosticsPlugin
- InputPlugin
- WindowPlugin
- AssetPlugin: load assets, AssetServer
- ScenePlugin
- RenderPlugin - with feature bevy_render
  - RenderApp
  - WindowRenderPlugin
  - CameraPlugin
    - CameraProjectionPlugin
  - ViewPlugin: `Res<Msaa>`
    - VisibilityPlugin
  - MeshPlugin: `Assets<Mesh>`,`RenderAssetPlugin<Mesh>`
  - GlobalsPlugin
  - FrameCountPlugin: update_frame_count
  - ImagePlugin: `Assets<Image>`
- SpritePlugin - with feature bevy_sprite
  - Mesh2dRenderPlugin
  - ColorMaterialPlugin: `Assets<ColorMaterial>`
- PbrPlugin - with feature bevy_pbr
- UiPlugin - with feature bevy_ui
- TextPlugin - with feature bevy_text
- AudioPlugin - with feature bevy_audio
- GilrsPlugin - with feature bevy_gilrs
- GltfPlugin - with feature bevy_gltf
- WinitPlugin - with feature bevy_winit

Stages after add_default_plugins

```rust
app.schedule.stage_order = [
    "first",
    "event_update",
    "scene",
    "load_assets",
    "pre_update",
    "update",
    "ui",
    "post_update",
    "asset_events",
    "render_resource",
    "render_graph_systems",
    "draw",
    "render",
    "post_render",
    "last",
]
```

To create a plugin we just need to implement the `Plugin` interface.

```rust
pub trait Plugin: Any + Send + Sync {
    fn build(&self, app: &mut AppBuilder);

    fn name(&self) -> &str { ... }
}
```

## Resource

We represent globally unique data using Resources.

Here are some examples data that could be encoded as Resources:

- Elapsed Time
- Asset Collections (sounds, textures, meshes)
- Renderers

Note that resources must come before components in the parameters list, or your function will not be convertible to a System.

```rust
struct GreetTimer(Timer);

fn greet_people(
    time: Res<Time>, mut timer: ResMut<GreetTimer>, person: &Person, name: &Name) {
    // update our timer with the time elapsed since the last update
    timer.0.tick(time.delta_seconds);

    // check to see if the timer has finished. if it has, print our message
    if timer.0.finished {
        println!("hello {}!", name.0);
        // reset the timer. it will start counting from 0
        timer.0.reset();
    }
}
```

The `delta_seconds` field on `Time` gives us the time that has passed since the last update.

## Queries

Queries give you direct control over entity iteration. They also provide a few extra filtering options, such as `With<Component>` and `Without<Component>` filters.

```rust
fn greet_people(
    time: Res<Time>, mut timer: ResMut<GreetTimer>, mut query: Query<(&Person, &Name)>) {
    timer.0.tick(time.delta_seconds);
    if timer.0.finished {
        for (_person, name) in &mut query.iter() {
            println!("hello {}!", name.0);
        }
        timer.0.reset();
    }
}
```

Limited to 65535 vertices or 79 subdivisions.

The coordinate system in Wgpu is based on DirectX and Metal's coordinate systems. That means that in normalized device coordinates the x axis and y axis are in the range of -1.0 to +1.0, and the z axis is 0.0 to +1.0.

wgpu's world coordinates have the y-axis pointing up, while texture coordinates have the y-axis pointing down.

Casting a ray from screen to 3D physics space is useful for object picking.
To cast a ray from the screen, you need a Camera node. A Camera can be in two projection modes: perspective and orthogonal. Because of this, both the ray origin and direction must be obtained.
