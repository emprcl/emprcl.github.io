+++
title = "Runal"
description = "a text-based creative coding environment for the terminal. It works similarly as processing or p5js and can either be programmed with JavaScript, or used as a Go package."
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

**Runal** is a text-based creative coding environment, that runs in the terminal.

It works similarly as [processing](https://processing.org/) or [p5js](https://p5js.org/) but it does all the rendering as text. It can either be programmed with [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript), or used as a [Go](https://go.dev/) package.

![runal screenshot](/images/runal/screenshot.png)

**Features**
- **Text only**: explore text and ascii art directly in the terminal
- **Familiar**: if you know [processing](https://processing.org/) or [p5js](https://p5js.org/), you already know Runal
- **Simple primitives**: provides a set of simple primitives for 2D shapes, trigonometry, randomization, colors...
- **Multi-language**: it can be programmed with Javascript, or used as a Go library
- **Fast feedback loop**: reloads your sketch each time you modify it
- **Export**: save your canvas to png images, gif animations or mp4 videos
- **Cross-platform**: runs on Linux, macOS, and Windows


## Installation

> _*If you want to use Runal as a Go package, see [Go package](#go-package).*_

**Runal** runtime is available for **Linux**, **macOS** and **Windows**.

### Quick-install

On **linux** or **macOS**, you can run this quick-install bash script:
```sh
curl -sSL empr.cl/get/runal | bash
```

### Manual installation

[Download the last release](https://github.com/emprcl/runal/releases) for your platform.

#### Linux & macOS

In your terminal:
```sh
# Extract files
mkdir -p runal && tar -zxvf signls_runal.tar.gz -C runal
cd runal

# Run runal
./runal

# Run runal demo
./runal -demo
```

#### Windows

> _*We recommend using [Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701) with a good monospace font like [Iosevka](https://typeof.net/Iosevka/) to display Runal correctly on Windows.*_

Unzip the archive and, in the same directory, run:
```winbatch
; Run runal
.\runal.exe

; Run runal demo
.\runal.exe -demo
```

Replace _./runal_ by _.\runal.exe_ for every following commands.

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

You can add extra methods `onKey` and `onMouse` to catch keyboard and mouse events:
```js
function onKey(c, e) {}
function onMouse(c, e) {}
````

And you can then execute the file with:
```sh
./runal -f sketch.js
```

The js file will be **automatically reloaded when modified**, no need to restart the command.

To **exit**, just type `ctrl+c`.

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
	runal.Run(context.Background(), setup, draw, onKey, onMouse)
}

func setup(c *runal.Canvas) {}

func draw(c *runal.Canvas) {}

// You can add extra methods `onKey` and `onMouse` to catch keyboard and mouse events
func onKey(c *runal.Canvas, e runal.KeyEvent) {}
func onMouse(c *runal.Canvas, e runal.MouseEvent) {}
```

And you can then execute the file with:
```sh
go run sketch.go
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

<hr class="separator"/>

<section class="reference">

### Properties

#### c.width
Returns the width of the canvas.

#### c.height
Returns the height of the canvas.

#### c.framecount
Number of frames rendered since the beginning.

#### c.isLooping
Boolean indicating whether the render loop is active.

#### c.mouseX <sub>since v0.5.0</sub>
Returns the horizontal mouse position relative to the terminal window.

#### c.mouseY <sub>since v0.5.0</sub>
Returns the vertical mouse position relative to the terminal window.

<hr class="separator"/>

### Objects

This is a list of different objects type returned by methods.

#### cell <sub>since v0.6.0</sub>
A object representing a single cell of the canvas.

Properties:
 - **char**: the character displayed on the cell
 - **background**: the string representation of the background color of the cell. [_See colors_](#colors)
 - **foreground**: the string representation of the foreground color of the cell. [_See colors_](#colors)

#### image <sub>since v0.6.0</sub>
An object representing an image containing [cells](#cell-since-v0-6-0).

Methods:
 - **cell(x, y)**: returns the [cell](#cell-since-v0-6-0) at (x, y) in the image.
 ```js

function setup(c) {
    let img = c.loadImage("my_image.jpg");

    // print the character of the cell at
    // position (1, 2) in the loaded image.
    console.log(img.cell(1, 2).char);
}
 ```


<hr class="separator"/>

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

#### c.get(x, y, w, h) <sub>since v0.6.0</sub>
Returns a part of the current canvas from origin (x, y) with width w and height h.
Returns an [image](#image-since-v0-6-0).

```js
// Get the entire canvas
let entireCanvas = c.get(0, 0, c.width, c.height);

// Get a 3x3 square of the canvas at position (2, 2)
let square = c.get(2, 2, 3, 3);
```

#### c.set(x, y, cells) <sub>since v0.6.0</sub>
Draws an [image](#image-since-v0-6-0) or a [cell](#cell-since-v0-6-0) to the canvas at (x, y).

```js
// Duplicate the first half of the canvas.
let half = c.get(0, 0, c.width/2, c.height);
c.set(c.width/2, 0, half);

// Duplicate a single cell
let single = c.get(0, 0, 1, 1);
c.set(1, 0, single.cell(0, 0));
```


<hr class="separator"/>

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

#### c.exit() <sub>since v0.6.0</sub>
Ends the program execution. It also quits the JavaScript runtime.

<hr class="separator"/>

### Colors

For every color arguments and properties (see [Draw](#draw), you can set 2 types of color values:
 - **hexadecimal** (_ex: `#000000`_): the hexadecimal color code
 - **ANSI 256** (_between 0 and 255_): one of the [256 ANSI colors](https://www.ditig.com/256-colors-cheat-sheet)

<hr class="separator"/>

### Draw

There are 3 types of zones when you draw:
 - **stroke**: a point, a line or the outline of a shape
 - **fill**: the inside of a closed shape
 - **background**: the background of the canvas

For each zones, you have 3 settings:
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

<hr class="separator"/>

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

<hr class="separator"/>

### Curve

#### c.bezier(x1, y1, x2, y2, x3, y3, x4, y4)
Draws a Bezier curve using four control points.

<hr class="separator"/>

### Math

All JavaScript [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) namespace objects are usable.

Runal **doesn't define trigonometry functions**, please use [_Math.sin()_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sin), [_Math.cos()_](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/cos) etc...

#### c.map(value, inputStart, inputEnd, outputStart, outputEnd)
Maps a value from one range to another.

#### c.dist(x1, y1, x2, y2)
Returns the euclidean distance between two points (x1, y1) and (x2, y2).

<hr class="separator"/>

### Random

#### c.random(min, max)
Returns a random float between min and max.

#### c.randomSeed(seed)
Sets the seed for random number generation.

<hr class="separator"/>

### Noise

#### c.noise1D(x)
Generates 1D Perlin noise (float number, range [0, 1]) for a given input.

#### c.noise2D(x, y)
Generates 2D Perlin noise (float number, range [0, 1]) for a given (x, y) coordinate.

#### c.noiseSeed(seed)
Sets the seed for noise generation.

#### c.loopAngle(duration)
Returns the angular progress (in radians, range [0, 2π]) through a looping cycle of given duration in seconds.

#### c.noiseLoop(angle, radius)
Returns a noise value (range [0, 1]) by sampling the noise on a circular path of the given radius. Angle is the loop angle in radians, from 0 to 2π. You can use [loopAngle](#c-loopangle-duration) for the angle value to control the loop duration.
This is useful for creating cyclic animations or evolving patterns that repeat perfectly after one full loop.

#### c.noiseLoop1D(angle, radius, x)
Returns a 1D noise value (range [0, 1]) that loops as the angle progresses. It samples a 2D noise space using the given radius and combines it with a horizontal offset.

#### c.noiseLoop2D(angle, radius, x, y)
Returns a 2D noise value (range [0, 1]) that loops as the angle progresses. It samples a circular path in 2D noise space, offset by the (x, y) coordinates.

<hr class="separator"/>

### Transform

#### c.translate(x, y)
Moves the origin of the canvas.

#### c.rotate(angle)
Rotates the drawing context by the given angle in radians.

#### c.scale(factor)
Scales the drawing context by the given factor.

<hr class="separator"/>

### Image

#### c.loadImage(path) <sub>since v0.6.0</sub>
Loads an image from the given path. Supported formats: jpg, png, webp.
Returns an [image](#image-since-v0-6-0).

You should usually load your images in the setup method:
```js

let img;

function setup(c) {
  img = c.loadImage("my_image.jpg");
}
```

#### c.image(image, x, y, w, h) <sub>since v0.6.0</sub>
Draws the given image at (x, y) with width w and height h.

```js
let img;

function setup(c) {
  img = c.loadImage("my_image.jpg");
}

function draw(c) {
  // draw the image on the entire canvas
  c.image(img, 0, 0, c.width, c.height);
}
```

<hr class="separator"/>

### Export

#### c.saveCanvasToPNG(filename)
Exports the current canvas to an image file (png).

#### c.saveCanvasToGIF(filename, duration)
Exports the current canvas to an animated gif file for a given duration (in seconds).

#### c.saveCanvasToPNG(filename, duration) <sub>since v0.4.0</sub>
> **This feature needs **[ffmpeg](https://ffmpeg.org/download.html)** installed**

Exports the current canvas to a mp4 (h264) video file for a given duration (in seconds).

#### c.savedCanvasFont(path)
Sets a custom font (tff) file used for rendering text characters in exported images generated via _SaveCanvasTo...()_ methods.

### Log

You can use JavaScript _console.log()_ to log things.

```js
let x = Math.osc(c.framecount);
console.log(x);
```

Entries will be written in a **console.log** file in your current directory.

You can display the logs live in another terminal pane with:
```sh
tail -f console.log
```

The **console.log** file is deleted upon exit.

### Events

#### onKey(c, event) <sub>since v0.5.0</sub>
Listen to keyboard events.

It's a root function, which means it should be placed at the same level as **setup()** and **draw()**.
```js
// mySketch.js
setup(c) { ... }

draw(c) { ... }

onKey(c, event) {
  // save the current canvas in a png file
  // when the "c" key is pressed.
  if (event.key == "space") {
    saveCanvasToPNG("canvas.png");
  }

  // same using the key code.
  if (event.code == 32) {
    saveCanvasToPNG("canvas.png");
  }
}
```

Event is an object with the following attributes:
 - **key**: the string representation of the key pressed (`a`, `b`, `c`, `1`, `2`, `3` etc...)
 - **code**: the numerical value of the key pressed (97, 98, 99, 49, 50, 51 etc...)

Key string representations for keys other than alphanumerical works as well. You can use the following strings to handle all your keyboard keys:
`enter`, `tab`, `backspace`, `esc`, `space`, `up`, `down`, `left`, `right`, `begin`, `find`, `insert`, `delete`, `select`, `pgup`, `pgdown`, `home`, `end`, `kpenter`, `kpequal`, `kpmul`, `kpplus`, `kpcomma`, `kpminus`, `kpperiod`, `kpdiv`, `kp0`, `kp1`, `kp2`, `kp3`, `kp4`, `kp5`, `kp6`, `kp7`, `kp8`, `kp9`.

You can also use `ctrl+a`, `alt+b` etc...

#### onMouse(c, event) <sub>since v0.5.0</sub>
Listen to mouse click events.

It's a root function, which means it should be placed at the same level as **setup()** and **draw()**.
```js
// mySketch.js
setup(c) { ... }

draw(c) { ... }

onMouse(c, event) {
  // draw a circle at mouse position when
  // the mouse left button is clicked.
  if (event.button == "left") {
    c.circle(event.x, event.y, 5);
  }
}
```
Event is an object with the following attributes:
 - **x**: the horizontal mouse position relative to the terminal window
 - **y**: the vertical mouse position relative to the terminal window
 - **button**: the mouse button that has been clicked. Possible values: **left**, **middle**, **right**

</section>
