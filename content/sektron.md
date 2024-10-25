+++
title = "sektron"
weight = 5
[extra]
release = "https://api.github.com/repos/emprcl/sektron/releases/latest"
+++

## Overview

<span id="release"></span> • 
[source code](https://github.com/emprcl/sektron) • 
[report an issue](https://github.com/emprcl/sektron/issues/new)

<hr/>

**Sektron** is a midi step sequencer, made with live performance in mind, that runs in the terminal (TUI). Its main purpose is to mimic the fun and immediacity of hardware sequencers by being entirely controllable via keyboard.

It's heavily inspired by [elektron devices](https://www.elektron.se).

![sektron screenshot](https://raw.githubusercontent.com/emprcl/sektron/refs/heads/main/docs/screenshot.png)


**Features**
 - Fully (and only) **controllable by keyboard**
 - **Customizable** keyboard mapping
 - Up to **10 midi tracks**, that can be attached to specific midi device and channel
 - Up to **128 steps per track**. The number of steps per track is independent, allowing complex polyrhythms
 - Parameters can be set per track or step (**parameter locking**)
 - Up to **64 patterns** can be loaded at the same time
 - **Pattern chaining**


## Installation

[Download the last release](https://github.com/emprcl/sektron/releases) for your platform.

Then:
```sh
# Extract files
mkdir -p sektron && tar -zxvf sektron_VERSION_PLATFORM.tar.gz -C sektron

# Run sektron
./sektron
```

You can also [build it yourself](https://github.com/emprcl/sektron?tab=readme-ov-file#build-it-yourself) if your want to.

## Usage

### Basic commands

```sh
# Run sektron
./sektron

# Display current version
./sektron --version
```

Hit `?` to see all keybindings. `esc` to quit.

### MIDI

**Sektron** doesn't generate sound on its own, but it works seamlessly with MIDI software or hardware. You can connect it to your favorite synthesizers, virtual instruments, or any MIDI-compatible devices for live performances or production.

Some companion apps that receive MIDI for testing Sektron:
 - [Enfer](https://neauoire.github.io/Enfer/) ([github](https://github.com/neauoire/Enfer))
 - [QSynth](https://qsynth.sourceforge.io/)

### Keyboard mapping

Keys mapping is fully customizable. After running Sektron for the first time, a `config.json` is created.
You can edit all the keys inside it.

You can select one of the default keyboard layouts available:
```sh
# QWERTY
./sektron --keyboard qwerty

# AZERTY
./sektron --keyboard azerty

# QWERTY MAC
./sektron --keyboard qwerty-mac

# AZERTY MAC
./sektron --keyboard azerty-mac
```

### Default keyboard mapping

The default key mapping looks like this:

![keyboard layout](https://raw.githubusercontent.com/emprcl/sektron/refs/heads/main/docs/keyboard-layout.png)

For qwerty keyboards, here's the default mapping:

 - `space` **play** or **stop**
 - `tab` **toggle parameter mode (track, record)**
 - `` ` `` **toggle pattern bank mode**
 - `1` `2` `3` `4` `5` `6` `7` `8` `9` `0` **select track**
 - `!` `@` `#` `$` `%` `^` `&` `*` `(` `)` **toggle track**
 - `q` `w` `e` `r` `t` `y` `u` `i` `q` `s` `d` `f` `g` `h` `j` `k` **select step** or **switch to pattern**
 - `Q` `W` `E` `R` `T` `Y` `U` `I` `Q` `S` `D` `F` `G` `H` `J` `K` **toggle step** or **chain pattern**
 - `,` **previous step**
 - `.` **next step**
 - `=` **add track**
 - `-` **remove track**
 - `+` **add step** to the selected track
 - `_` **remove step** from the selected track
 - `p` **page up** either steps or patterns if more than 16 items
 - `;` **page down** either steps or patterns if more than 16 items
 - `shift`+`up` **increase tempo**
 - `shift`+`down` **decrease tempo**
 - `ctrl`+`c` **copy selected step**
 - `ctrl`+`v` **paste selected step**
 - `ctrl`+`up` **add new midi control** to the selected track
 - `ctrl`+`down` **remove midi control**. It will remove the selected one
 - `enter` **validate selection**
 - `left` **move parameter selection to the left**
 - `right` **move parameter selection to the right**
 - `up` **increase selected parameter value**
 - `down` **decrease selected parameter value**
 - `?` **show help**
 - `escape` or `ctrl`+`q` **quit**

### Pattern bank

You manage your projects using a **bank**. When you start the program, you can provide a **bank** JSON file, or if none is provided, a default file (_default.json_) will be created or loaded automatically. Each bank can store up to **128 patterns**.

```sh
./sektron --bank my-patterns.json
```

The patterns are **saved automatically** whenever you changes pattern or exit the program, so you never have to worry about losing progress.
