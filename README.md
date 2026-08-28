# IvyCompositor

**IvyCompositor** is a small, modular **Wayland compositor** written in **C**, built on top of [wlroots](https://gitlab.freedesktop.org/wlroots/wlroots).

The project is an exploration of Wayland compositor development and Linux graphics infrastructure, with an emphasis on keeping the implementation understandable and separating compositor responsibilities into focused modules.

IvyCompositor is inspired by the structure of [tinywl](https://gitlab.freedesktop.org/wlroots/wlroots/-/tree/master/tinywl) while growing into its own modular architecture.

## Features

The current implementation includes components for:

- **Wayland compositor core** and server lifecycle
- **Output management**
- **Scene management**
- **Keyboard and pointer input**
- **Seat management**
- **Cursor handling and cursor-shape support**
- **Idle inhibition**
- **XDG shell / toplevel surfaces**
- **Layer-shell surfaces**
- **XWayland integration**
- **XWayland surface handling**
- **Wayland protocol header generation** during the build

The compositor is structured so that core, input, and shell responsibilities live in separate modules rather than being concentrated in a single implementation file.

## Architecture

```text
IvyCompositor
├── Core
│   ├── Wayland display
│   ├── wlroots backend
│   ├── renderer / allocator
│   ├── outputs
│   ├── scene
│   └── server lifecycle
│
├── Input
│   ├── Seat
│   ├── Keyboard
│   ├── Cursor
│   ├── Cursor shape
│   └── Idle inhibitor
│
└── Shell
    ├── XDG shell
    │   ├── Toplevels
    │   ├── Popups
    │   ├── Decorations
    │   ├── Icons
    │   └── Activation
    ├── Layer shell
    └── XWayland
        └── Surfaces
```

The top-level server initializes the compositor core, output manager, scene, shell, input, and idle handling before entering the Wayland event loop. Shutdown reverses that ownership relationship and destroys the corresponding wlroots and Wayland resources. 

## Project Structure

```text
IvyCompositor/
├── include/
│   ├── core/
│   ├── input/
│   └── shell/
│
├── src/
│   ├── core/
│   │   ├── core.c
│   │   ├── output.c
│   │   ├── scene.c
│   │   └── server.c
│   │
│   ├── input/
│   │   ├── cursor/
│   │   ├── idle_inhibitor.c
│   │   ├── input.c
│   │   ├── keyboard.c
│   │   └── seat.c
│   │
│   ├── shell/
│   │   ├── layer/
│   │   ├── xdg/
│   │   ├── xwayland/
│   │   └── shell.c
│   │
│   └── main.c
│
└── CMakeLists.txt
```

## Technology

- **Language:** C11
- **Display protocol:** Wayland
- **Compositor framework:** wlroots 0.20
- **Input:** xkbcommon
- **XWayland support:** XCB, XCB-ICCCM, XCB-EWMH
- **Build system:** CMake + pkg-config
- **Protocol generation:** `wayland-scanner`

The build configuration explicitly discovers Wayland Server, wlroots 0.20, xkbcommon, and the XCB libraries, and generates protocol headers for `xdg-shell`, cursor-shape, layer-shell, and output power-management protocols. fileciteturn26file0

## Building

IvyCompositor currently targets Linux systems with a Wayland/wlroots development environment installed.

### Dependencies

You need development packages providing at least:

- C compiler with C11 support
- CMake 3.10+
- pkg-config
- wayland-server
- wlroots 0.20
- xkbcommon
- xcb
- xcb-icccm
- xcb-ewmh
- wayland-scanner
- wayland-protocols
- wlr-protocols

The exact package names vary between Linux distributions.

### Build

```bash
cmake -S . -B build
cmake --build build
```

The resulting executable is produced by the standard CMake build tree.

## Running

IvyCompositor is a system-level application and should be run from a suitable Linux session or TTY rather than launched like a normal desktop application.

For development, inspect the wlroots and Wayland environment available on your system before starting the compositor.

The server creates a Wayland display/socket, starts the wlroots backend, sets `WAYLAND_DISPLAY`, optionally launches a startup command, and enters the Wayland event loop. fileciteturn25file0

## Learning Focus

IvyCompositor is primarily a systems-programming and graphics-infrastructure project. It is a practical way to explore:

- Wayland compositor architecture
- wlroots APIs and compositor backends
- Linux display stacks
- Rendering/output management
- Input and seat handling
- Wayland protocol lifecycles
- XDG shell semantics
- Layer-shell surfaces
- XWayland integration
- Event-driven C architecture
- Resource ownership and cleanup
- Generating and consuming Wayland protocol headers

## Design Goals

The project deliberately favors:

- **Small modules** over a monolithic compositor implementation
- **Explicit ownership** of compositor resources
- **Direct interaction with wlroots and Wayland APIs**
- **Minimal abstraction** where it does not provide practical value
- **Readable code** that can be used as a learning reference

## Status

IvyCompositor is an experimental personal project. The compositor architecture is still evolving as I learn more about Wayland, wlroots, Linux graphics, and compositor design.

## License

No repository license is currently declared. Check the repository for the latest licensing information before redistributing the project.
