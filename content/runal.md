+++
title = "Runal"
description = "a simple creative coding environment for the terminal. It works similarly as processing or p5js and can either be programmed with JavaScript, or used as a Go package."
weight = 1
[extra]
release = "https://api.github.com/repos/emprcl/runal/releases/latest"
download = "https://github.com/emprcl/runal/releases/latest"
image = "/images/runal/screenshot.png"
+++

## Overview

<span id="release"></span> •
[source code](https://github.com/emprcl/runal) •
[report an issue](https://github.com/emprcl/runal/issues/new)

<hr/>

**Runal** is a simple creative coding environment for text and ascii art, that runs in the terminal.

It works similarly as [processing](https://processing.org/) or [p5js](https://p5js.org/) and can either be programmed with [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), or used as a [Go](https://go.dev/) package.

![runal screenshot](/images/runal/screenshot.png)

**Features**
- **Text only**: explore text and ascii art directly in the terminal
- **Familiar**: if you know [processing](https://processing.org/) or [p5js](https://p5js.org/), you already know Runal
- **Simple primitives**: provides a set of simple primitives for 2D shapes, trigonometry, randomization, colors...
- **Multi-language**: it can be programmed with Javascript, or used as a Go library
- **Fast feedback loop**: reloads your sketch each time you modify it
- **Cross-platform**: runs on Linux, macOS, and Windows


## Installation (runtime)

> _*If you want to use Runal as a Go package, see [Go package](#go-package).*_

**Runal** runtime is available for **Linux**, **macOS** and **Windows**.

[Download the last release](https://github.com/emprcl/runal/releases) for your platform.

### Linux & macOS

In your terminal:
```sh
# Extract files
mkdir -p runal && tar -zxvf signls_runal.tar.gz -C runal
cd runal

# Run runal
./runal -f my_sketch.js
```

### Windows

> _*We recommend using [Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701) with a good monospace font like [Iosevka](https://typeof.net/Iosevka/) to display Runal correctly on Windows.*_

Unzip the archive and, in the same directory, run:
```
.\runal.exe -f my_sketch.js
```
Replace _./runal_ by _.\runal.exe_ for every following commands.

### Go install

If you're a Go developer, you can install it via _go install_:
```sh
go install github.com/emprcl/runal@latest
```

### Build it yourself

You can also [build it yourself](https://github.com/emprcl/runal?tab=readme-ov-file#build-it-yourself) if your want to.

## Usage

### JavaScript runtime

You will use JavaScript for scripting your sketch. Your js file should contain a setup and a draw method. Both methods take a single argument (here c) representing a canvas object that holds all the available primitives:
```JavaScript
// sketch.js
function setup(c) {}

function draw(c) {}
```

And you can then execute the file with:
```sh
./runal -f sketch.js
```

The js file will be **automatically reloaded when modified**, no need to restart the command.

### Go package

Because **Runal** is written in Go, you can also use it as a Go package.
```Go
// sketch.go
package main

import (
	"context"
	"os"
	"os/signal"

	"github.com/emprcl/runal"
)

func main() {
	runal.Run(context.Background(), setup, draw, onKey)
}

func setup(c *runal.Canvas) {}

func draw(c *runal.Canvas) {}

func onKey(c *runal.Canvas, key string) {}
```

## Examples

### Perlin noise map

```js
// perlin_noise_map.js

function setup(c) {
  // Here we're just saying to print each cells 2 times
  // just to get a more balenced output.
  // A terminal cell is not a square, so the result can feel
  // squished a little bit.
  c.cellPaddingDouble();
}

function draw(c) {
  // For each cell of the canvas, we map a perlin noise value (between 0 and 1)
  // to an ansi color (between 150 and 231).
  // Adding c.framecount in the noise parameter makes the canvas move over the map.
  for (let i = 0; i < c.width; i++) {
    for (let j = 0; j < c.height; j++) {
      let color = c.map(
        c.noise2D(
          i * 0.009 + c.framecount / 1000,
          j * 0.009 + c.framecount / 1000,
        ),
        0,
        1,
        150,
        231,
      );
      // We will use "§" character for drawing,
      // width the computed foreground color, and a black background.
      c.stroke("§", color, "#000000");
      c.point(i, j);
    }
  }
}

function onKey(c, key) {
  // When hitting the "c" key, we save the current
  // canvas in a png file.
  if (key == "c") {
    c.saveCanvas(`canvas_${Date.now()}.png`);
  }
  // When hitting the space keep, we update the noise seed
  // to get a new map.
  if (key == " ") {
    c.noiseSeed(Date.now());
    c.redraw();
  }
}
```

### More

You can find more small examples in the [Github repository](https://github.com/emprcl/runal/tree/main/examples).

## Reference

This documentation covers the full JavaScript API.

You're missing a specific method or feature? Feel free to [open an issue](https://github.com/emprcl/runal/issues/new).


> _Go properties and methods are the same as JavaScript, with an uppercase first letter (js: c.width, go: c.Width)._

---

### Properties

#### c.width
Returns the width of the canvas in cells.

#### c.height
Returns the height of the canvas in cells.

#### c.framecount
Number of frames rendered since the beginning.

#### c.isLooping
Boolean indicating whether the render loop is active.

---

### Canvas

Drawing in the terminal is a little bit weird, because each cell is not a square. Therefore, **results can look squished** depending on what you're trying to do.

One simple way to fix this problem is to **use 2 cells instead of one** for drawing one canvas pixel, but the question is : which character to draw in the second cell?

You can control this behavior with 2 methods:
 - **c.cellPadding(char)**: it allows you to define which character to use in the second cell. One obvious option is to use a black space, but other choice may give you fun results.
 - **c.cellPaddingDouble()**: it just duplicates the first character.


#### c.cellPadding(char)
Sets a character used for cell spacing between elements.

#### c.cellPaddingDouble()
Makes every cell duplicated.

#### c.clear()
Clears the canvas.

#### c.size(w, h)
Resizes the canvas.

---

### Colors

For every color arguments (see [Stroke, Fill and Background](#stroke-fill-and-background), you can set 2 types of color values:
 - **hexadecimal** (_ex: `#000000`_): the hexadecimal color code
 - **ANSI 256** (_between 0 and 255_): one of the [256 ANSI colors](https://www.ditig.com/256-colors-cheat-sheet)

---

### Stroke, Fill and Background

There are 3 types of zones when you draw:
 - **stroke**: a point, a line or the outline of a shape
 - **fill**: the inside of a closed shape
 - **background**: the background of the canvas

For each zones, you have 2 settings:
 - **text**: the unicode character set to use when drawing
 - **foreground**: the color of the text
 - **background**: the color of the background of the cell

It's a little bit confusing but it's very powerful.

If you have a better API suggestion, please [create an issue](https://github.com/emprcl/runal/issues/new).

#### c.fill(text, foregroundColor, backgroundColor)
Sets the fill style used to render shapes.

#### c.fillText(text)
Sets only the fill character.

#### c.fillFg(color)
Sets only the foreground color for fill.

#### c.fillBg(color)
Sets only the background color for fill.

#### c.noFill()
Disables fill rendering.

#### c.stroke(text, foregroundColor, backgroundColor)
Sets the stroke style used to outline shapes.

#### c.strokeText(text)
Sets only the stroke character.

#### c.strokeFg(color)
Sets only the foreground color for stroke.

#### c.strokeBg(color)
Sets only the background color for stroke.

#### c.background(text, foregroundColor, backgroundColor)
Sets the background fill style with custom character and colors.

#### c.backgroundText(text)
Sets the character used for the background.

#### c.backgroundFg(color)
Sets the foreground color for the background style.

#### c.backgroundBg(color)
Sets the background color for the background style.

---

### Shapes


#### c.text(str, x, y)
Draws a string at position (x, y).

#### c.point(x, y)
Draws a point at the given position.

#### c.line(x1, y1, x2, y2)
Draws a straight line between two points.

#### c.circle(x, y, r)
Draws a circle centered at (x, y) with the given radius r.

#### c.ellipse(x, y, rx, ry)
Draws an ellipse centered at (x, y) with radiuses rx and ry.

#### c.rect(x, y, w, h)
Draws a rectangle starting at (x, y) with width w and height h.

#### c.square(x, y, size)
Draws a square with the given top-left corner and side length.

#### c.triangle(x1, y1, x2, y2, x3, y3)
Draws a triangle using three vertex points.

#### c.quad(x1, y1, x2, y2, x3, y3, x4, y4)
Draws a quadrilateral defined by four points.

---

### Curve

#### c.bezier(x1, y1, x2, y2, x3, y3, x4, y4)
Draws a Bezier curve using four control points.

---

### Control

#### c.loop()
Starts the draw loop if stopped.

#### c.noLoop()
Stops the draw loop if started.

#### c.redraw()
Forces one frame render in noLoop mode.

#### c.fps(fps)
Sets the desired frames per second.

#### c.push()
Saves the current drawing state (stroke, fill, background, rotate, translate, scale).

#### c.pop()
Restores the last saved drawing state (stroke, fill, background, rotate, translate, scale).

---

### Math

All JavaScript [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) namespace objects are usable.

Runal **doesn't expose specific trigonometry functions**, please use _Math.sin()_, _Math.cos()_ etc...

#### c.map(value, inputStart, inputEnd, outputStart, outputEnd)
Maps a value from one range to another.

#### c.dist(x1, y1, x2, y2)
Returns the euclidean distance between two points (x1, y1) and (x2, y2).

---

### Randomness

#### c.random(min, max)
Returns a random float between min and max.

#### c.randomSeed(seed)
Sets the seed for random number generation.

---

### Noise

#### c.noise1D(x)
Generates 1D Perlin noise for a given input.

#### c.noise2D(x, y)
Generates 2D Perlin noise for a given (x, y) coordinate.

#### c.noiseSeed(seed)
Sets the seed for noise generation.

---

### Transformations

#### c.translate(x, y)
Moves the origin of the canvas.

#### c.rotate(angle)
Rotates the drawing context by the given angle in radians.

#### c.scale(factor)
Scales the drawing context by the given factor.

---

### Export

#### c.saveCanvas(filename)
Exports the current canvas to an image file (png).

#### c.savedCanvasFont(path)
Sets a custom font (tff) file used for rendering text characters in exported images generated via _SaveCanvas()_.
