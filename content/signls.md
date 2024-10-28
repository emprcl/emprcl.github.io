+++
title = "signls"
description = "a non-linear, generative MIDI sequencer designed for music composition and live performances, all within the terminal"
weight = 1
[extra]
release = "https://api.github.com/repos/emprcl/signls/releases/latest"
image = "/images/signls/screenshot.png"
+++

## Overview

<span id="release"></span> •
[source code](https://github.com/emprcl/signls) •
[report an issue](https://github.com/emprcl/signls/issues/new)

<hr/>

**Signls** (_pronounced signals_) is a non-linear, generative MIDI sequencer designed for music composition and live performances, all within the terminal. It allows you to create complex, evolving musical patterns using a grid-based approach. You can place nodes on the grid, and these nodes can emit signals, relay them, or trigger MIDI notes. There are 9 different types of nodes to explore, each with its own unique behavior.

With Signls, you can generate dynamic, generative music, meaning that the patterns evolve and change over time. It's designed to give you a powerful creative tool to build intricate sequences without being stuck in a rigid timeline or structure.

It takes inspiration from [Orca](https://100r.co/site/orca.html) and [Nodal](https://nodalmusic.com/).

<iframe style="width:100%; aspect-ratio: 16 / 9;" src="https://www.youtube-nocookie.com/embed/N2jTlwaZbgk?si=P2ymGjp82mqf9--h" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Features**
- **Non-linear sequencing**: unlike traditional sequencers, Signls doesn't force you into a single direction. Your sequences can move and shift in multiple ways, allowing for complex and unique arrangements.
- **Randomize everything**: create evolving musical patterns that shift over time, adding depth and unpredictability to your compositions.
- **Live performance**: designed to be used in real-time, making it a adequate tool for live performances where improvisation is key.
- **Keyboard first**: Signls operates directly from your terminal, giving you control in a simple, lightweight environment, where everything is controllable via keyboard.
- **Cross-platform**: runs on Linux, macOS, and Windows

## Installation

**Signls** is available for **Linux**, **macOS** and **Windows**.

[Download the last release](https://github.com/emprcl/signls/releases) for your platform.

Then, if your terminal:
```sh
# Extract files
mkdir -p signls && tar -zxvf signls_VERSION_PLATFORM.tar.gz -C signls

# Run signls
./signls
```

You can also [build it yourself](https://github.com/emprcl/signls?tab=readme-ov-file#build-it-yourself) if your want to.

## Usage

### Basic commands

```sh
# Run signls
./signls

# Display current version
./signls --version
```

Hit `?` to see all keybindings. `esc` to quit.


### Keyboard mapping

Keys mapping is fully customizable. After running signls for the first time, a _config.json_ is created.
You can edit all the keys inside it.

You can select one of the default keyboard layouts available:
```sh
# QWERTY
./signls --keyboard qwerty

# AZERTY
./signls --keyboard azerty

# QWERTY MAC
./signls --keyboard qwerty-mac

# AZERTY MAC
./signls --keyboard azerty-mac
```

Key binding reference:
 - [qwerty](https://github.com/emprcl/signls/blob/7d9c8016e99fc9c973f61764fb9801d92eee21db/filesystem/keymap.go#L183)
 - [qwerty mac](https://github.com/emprcl/signls/blob/7d9c8016e99fc9c973f61764fb9801d92eee21db/filesystem/keymap.go#L244)
 - [azerty](https://github.com/emprcl/signls/blob/7d9c8016e99fc9c973f61764fb9801d92eee21db/filesystem/keymap.go#L62)
 - [azerty mac](https://github.com/emprcl/signls/blob/7d9c8016e99fc9c973f61764fb9801d92eee21db/filesystem/keymap.go#L123)

### MIDI

**Signls** doesn't generate sound on its own, but it works seamlessly with MIDI software or hardware. You can connect it to your favorite synthesizers, virtual instruments, or any MIDI-compatible devices for live performances or production.

Press the `f2` key to cycle through the available MIDI drivers and set up your preferred MIDI output.

Some companion apps that receive MIDI for testing Signls:
 - [Enfer](https://neauoire.github.io/Enfer/) ([github](https://github.com/neauoire/Enfer))
 - [QSynth](https://qsynth.sourceforge.io/)

## Workflow

### User Interface

<img src="/images/signls/ui_1.png" alt="UI 1" />

1. **Grid**: The [grid](#grid) is where you place [nodes](#nodes). Each nodes displays its type and emit/relay directions. Signals are displayed in white.
2. **Selection indicator**: Shows the currently selected [nodes](#nodes).
3. **Mode indicator**: Shows the current mode - _move_, _[edit](#parameters)_ or _[bank](#bank)_.
4. **Selector position**: Shows the current selector position.
5. **Grid size**: Shows the current grid size. Useful to know if the grid is bigger than the current terminal window.
6. **Tempo**: Shows the current [tempo](#timing) in bpm (_beats per minute_).
7. **Play status**: Shows if the grid is currently playing (▶) or stopped (■). Also shows the number of 1/16 notes since it started to play.
8. **Root key**: Shows the current [root key](#transposition).
9. **Scale**: Shows the current [scale](#transposition).
10. **Bank**: Shows the currently selected grid in the [bank](#bank), and the name of the bank (which is the bank filename).
11. **MIDI device**: Shows the currently selected [MIDI device](#midi).
12. **Control zone**: Shows either [grid](#grid) informations and parameters (_move_ mode), selected [node parameters](#parameters) (_edit_ mode) or [bank](#bank) grid slots (_bank_ mode).


### Grid

The **grid** serves as a canvas for your sequencing, where you control the flow of MIDI signals across various [nodes](#nodes). You can **start** or **stop** the grid's underlying sequencer by pressing `space`.

By default, the **size** of the grid adapts to your terminal size. If you **increase** the terminal window, the grid will expand accordingly. However, if you **decrease** the terminal size, the grid remains unchanged to prevent the accidental loss of nodes outside the visible bounds. You can still scroll through the grid even if the terminal is smaller.

If you want to force the grid to **resize** and match the terminal (which may result in some nodes being deleted), you can press `f10` to do so manually.

### Nodes

**Nodes** can perform three main functions based on their type:
 - they can **emit new signals**
 - they can **relay incoming signals** to up to 4 directions
 - they can **trigger MIDI messages**

To **add** nodes on the grid, move the cursor using the arrow keys and press keys `1` to `9` to choose one of the **[9 available node types](#nodes-reference)**.

To **remove** nodes from the grid, move the cursor using the arrow keys and press `backspace`.

You can manually **trigger** a node by using `/`.

You can **mute** nodes to temporarily silence their behavior:
- Press `m` to toggle mute on the selected nodes.
- Press `M` to force mute sync across all selected nodes, ensuring they are all muted or unmuted together.

This is useful for controlling which nodes are active during live performances or while experimenting with different parts of your sequence.

You can easily manage nodes on the grid by copying, cutting, and pasting them using the usual key bindings:
 - `ctrl`+`C` to **copy**,
 - `ctrl`+`X` to **cut**
 - `ctrl`+`V` to **paste**.

 To move nodes in bulk, you can **select multiple nodes** by holding `shift` + `↑` `↓` `←` `→` to define a selection area. This makes it easy to reposition or replicate parts of your sequence.

#### Emit and relay directions

A key feature of each node is the direction in which it **emits** or **relays signals**. You can configure up to four directions: **up**, **down**, **left**, and **right**. To modify a node's directions, move the cursor to the desired node and press `Ctrl` + `↑` `↓` `←` `→` to add or remove directions. The way a node uses these directions (one or multiple) depends on its specific behavior.

<img src="/images/signls/directions.jpg" alt="directions" class="shadow" width="300"/>

#### Parameters

Each node has adjustable parameters that you can edit to modify its behavior. To enter **node editing mode**, move the cursor to the node you want to modify and press `enter`. The available parameters will appear in the control bar at the bottom. You can navigate between parameters using `←` `→` keys. To change a parameter value, press `ctrl`+`↑` to increase or `ctrl`+`↓` to decrease it.

Each node parameter can have up to four alternative values:
 - **Main 1** `Ctrl`+`↑`/`↓`: adjusts the main value
 - **Main 2** `Ctrl`+`←`/`→`: adjusts a second value, often used for randomization
 - **Alt 1** `Shift`+`↑`/`↓`: adjusts a third alternative value
 - **Alt 2** `Shift`+`←`/`→`: adjusts a fourth alternative value

> _We will refer to these as **Main 1**, **Main 2**, **Alt 1**, and **Alt 2** for simplicity_

Each node (except "The Hole") shares **five common MIDI parameters**.

<img src="/images/signls/ui_2.png" alt="UI 2" />

Parameters are displayed in two parts if an alternative value (for example [randomization](#randomization)) is set:
1. **the actual value**: here _F5_ for the key or _100_ for the velocity
2. **the alternative value**: here _+8_ for the key or _-18_ for the velocity

<br/>


**Key**: key of the MIDI note
- **Value Range**: A1 - G10
- **Main 1**: key value for the note
- **Main 2**: randomization range
- **Alt 1**: _unused_
- **Alt 2**: note modes (random | silent)

**Velocity**: intensity of the MIDI note
- **Value Range**: 0 - 127
- **Main 1**: velocity value for the note
- **Main 2**: randomization range
- **Alt 1**: _unused_
- **Alt 2**: _unused_

**Length**: duration of the MIDI note.
- **Value Range**: 1/64 - inf
- **Main 1**: length value of the note
- **Main 2**: randomization range
- **Alt 1**: _unused_
- **Alt 2**: _unused_

**Channel**: MIDI channel
- **Value Range**: 1 - 16
- **Main 1**: channel value
- **Main 2**: randomization range
- **Alt 1**: _unused_
- **Alt 2**: _unused_

**Probability**: the chance of triggering the MIDI note
- **Value Range**: 0 - 100
- **Main 1**: probability value
- **Main 2**: _unused_
- **Alt 1**: _unused_
- **Alt 2**: _unused_

> _You can edit multiple nodes at once by selecting them together.The common parameters for all selected nodes will be displayed, and any changes you make will apply to all of them simultaneously._

### Timing

Each position on the grid represents a **1/16 note**. You can adjust the **tempo** (in beats per minute, or BPM) to control how fast signals move across the grid. To modify the tempo, simply press `=` to increase the BPM or `-` to decrease it, allowing you to set the pace of your sequence in real time.

### Transposition

The MIDI notes assigned to each node are fixed. However, when the **root key** or **scale** of the grid is changed, these notes are **transposed** according to the new root key and scale. This means the original note values are shifted in relation to the grid's updated musical context, allowing you to easily adjust the overall harmony without manually changing the notes on each node. The transposition happens relative to the set root and scale, providing a flexible way to experiment with different keys and tonalities.

### Randomization

You can apply **randomization** to most node parameters. When editing randomization, you can specify positive or negative randomization values.

For example, if you set the **velocity** to _80+5_, the random value will be picked between 80 and 85. If you set the velocity to _80-5_, the random value will be picked between 75 and 80. This allows for subtle variations in your sequences, adding a layer of unpredictability while keeping control over the range of changes.

> _Randomized node keys will always conform to the current [scale](#transposition)._

### Bank

You manage your projects using a **bank**. When you start the program, you can provide a **bank** JSON file, or if none is provided, a default file (_default.json_) will be created or loaded automatically. Each bank can store up to **32 grids**.

```sh
./signls --bank my-grids.json
```

The grids are **saved automatically** whenever you make changes or exit the program, so you never have to worry about losing progress.

To load a specific grid from the bank, press `tab` to switch to the **bank view**, then use the arrow keys to select a grid slot and press `enter` to load it. This allows you to quickly swap between different configurations during live performances or while working on different projects.

<img src="/images/signls/ui_3.png" alt="UI 3" />

The _&#800;_ character under the grid number indicates that the grid slot is not empty.

## Nodes reference

Here is a reference guide for all the **node types** available in **Signls**. Each node has common [**note parameters**](#parameters) (except for the Hole) like key, velocity, length, channel, and probability. Some nodes also have **extra parameters** that give them unique behavior.

### <img src="/images/signls/bang.jpg" alt="bang" class="node shadow" width="25"/> **Bang**
- **Description:** emits a signal when the grid starts playing and relays signals on all configured directions
- **Key binding**: `1`
- **Extra Parameters:** none

### <img src="/images/signls/euclid.jpg" alt="euclid" class="node shadow" width="25"/> **Euclid**
- **Description:** emits signals based on the **euclidean rhythm algorithm**, ensuring an even distribution of steps across the grid. Relays signals on all configured directions
- **Key binding**: `2`
- **Extra Parameters:**
  - **Steps:** number of total steps in the pattern
	- **Value Range**: 1 - 128
	- **Main 1**: steps value
	- **Main 2**: randomization range
	- **Alt 1**: _unused_
	- **Alt 2**:  _unused_
  - **Triggers:** number of signals to emit within the total steps
	- **Value Range**: 1 - 128
	- **Main 1**: triggers value
	- **Main 2**: randomization range
	- **Alt 1**: _unused_
	- **Alt 2**:  _unused_
  - **Offset:** shifts the start point of the pattern
	- **Value Range**: 0 - 128
	- **Main 1**: offset value
	- **Main 2**: randomization range
	- **Alt 1**: _unused_
	- **Alt 2**:  _unused_

### <img src="/images/signls/pass.jpg" alt="pass" class="node shadow" width="25"/> **Pass**
- **Description:** passes signals through without affecting their direction. No direction configuration is possible
- **Key binding**: `3`
- **Extra Parameters:** none

### <img src="/images/signls/spread.jpg" alt="spread" class="node shadow" width="25"/> **Spread**
- **Description:** relays signals on all configured directions, distributing them evenly
- **Key binding**: `4`
- **Extra Parameters:** None

### <img src="/images/signls/cycle.jpg" alt="cycle" class="node shadow" width="25"/> **Cycle**
- **Description:** relays signals in a **clockwise direction**, starting from the "up" direction, one at a time
- **Key binding**: `5`
- **Extra Parameters:** none

### <img src="/images/signls/dice.jpg" alt="dice" class="node shadow" width="25"/> **Dice**
- **Description:** relays signals in a randomly selected direction each time it is triggered
- **Key binding**: `6`
- **Extra Parameters:** none

### <img src="/images/signls/toll.jpg" alt="toll" class="node shadow" width="25"/> **Toll**
- **Description:** relays signals on all configured directions, but only after being triggered a specific number of times
- **Key binding**: `7`
- **Extra Parameters:**
  - **Threshold:** the number of times the node must be triggered before it relays a signal
	- **Value Range**: 1 - no upper limit
	- **Main 1**: offset value
	- **Main 2**: randomization range
	- **Alt 1**: _unused_
	- **Alt 2**:  _unused_

### <img src="/images/signls/zone.jpg" alt="zone" class="node shadow" width="25"/> **Zone**
- **Description:** relays signals on all configured directions and immediately propagates the trigger to all neighboring nodes, making it ideal for triggering chords
- **Key binding**: `8`
- **Extra Parameters:** none

### <img src="/images/signls/hole.jpg" alt="hole" class="node shadow" width="25"/> **Hole**
- **Description:** instantly teleports the signal to a specified location on the grid without triggering any notes
- **Key binding**: `9`
- **Extra Parameters:**
  - **Destination:** The coordinate of the destination
	- **Value Range**: 1 - grid width/height
	- **Main 1**: y-coordinate value of the destination
	- **Main 2**: x-coordinate value of the destination
	- **Alt 1**: randomization range for the y-coordinate
	- **Alt 2**: randomization range for the y-coordinate
