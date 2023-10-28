Paintfuck
=====

Paintfuck is a variation of Brainfuck, which operates on a canvas of paint
rather than a tape of numbers.

## Commands

- `<`: Move the data pointer left. Wraps around edges.
- `>`: Move the data pointer right. Wraps around edges.
- `^`: Move the data pointer up. Wraps around edges.
- `v`: Move the data pointer down. Wraps around edges.
- `+`: Increment the value of the cell at the data pointer position. Wraps from 0 to 255.
- `-`: Decrement the value of the cell at the data pointer position. Wraps from 0 to 255.
- `[`: Repetition block start: If the byte at the current data pointer position
  0, the instruction pointer will jump to the matching `]` command. If not, the 
  instruction pointer will increment as normal.
- `[`: Repetition block start: If the byte at the current data pointer position
  is not 0, the instruction pointer will jump to the matching `[` command. If 
  it is, the instruction pointer will increment as normal.

[![Paintfuck color stripes][IMG0]][DEMO1]

**[See Demo][DEMO1]**

**[Draw Now][DRAW1]**


Contents
--------

* [Introduction](#introduction)
* [Implementation](#implementation)
* [Get Started](#get-started)
* [Demos](#demos)
* [Canvas](#canvas-tape-and-color)
* [Commands](#commands)
* [Code Normalisation and Validation](#code-normalisation-and-validation)
* [License](#license)
* [Support](#support)


Introduction
------------

Paintfuck is a mashup of [CFR\[\]](https://susam.net/cfr.html#) and 
[Brainfuck](https://en.wikipedia.org/wiki/Brainfuck).

This project introduces the Paintfuck language and provides a web-based
implementation using HTML5 Canvas and JavaScript.


Implementation
--------------

The current stable version of a Paintfuck] implementation is available at
the following links:

* [kjeld-schmidt.com/assets/pages/paintfuck.html][DRAW1]

[DRAW1]: https://kjeld-schmidt.com/assets/pages/paintfuck.html

[DEMO1]: https://kjeld-schmidt.com/assets/pages/paintfuck.html#VlZWVj4rKysrKysrWzwrKysrKysrKz4tXTxbPj4+KysrKysrK1s8KysrKysrKys+LV1eW1YrXi1dVitbWz5dPFstPis+Kzw8XT4+Wy08PCs+Pl08K1s8XT4tXTw8LVstViteXVZd
[DEMO3]: https://kjeld-schmidt.com/assets/pages/paintfuck.html#Pj4+Pj4rPDw8PDw8LVstWz4tPCs+XStWPitWPitWPFs+LTwrPl0rVjwrVjwrXQ==

[IMG0]: https://kjeld-schmidt.com/assets/images/paintfuck/demo-1.png
[IMG3]: https://kjeld-schmidt.com/assets/images/paintfuck/demo-6.png


Get Started
-----------

Enter the following code in the input field to create some colored pixels. 
I encourage you to type it out character for character to get a simple feel for
how it operates.

```
+>>++>>>>+++vvvvvv++++<<<<+++++<<++++++^^^>>>>-
```

The following code uses a loop to fill an entire horizontal line. It 
accomplishes this by setting the first cell to -1 (=255), then moving right and 
incrementing each cell until the field becomes 0 _after_ adding to it - which is
the case for our first cell. The pointer has to wrap around the edge for that.

```
-[>+]+
```

As a simple check of understanding, try drawing a vertical and diagonal line, 
and giving them different colors.

As your first challenge, try to think about how you can create a line segment of 
arbitrary length, i.e. 3, 10 or 50 pixels, making it dependent only on the value
of the first cell.

To get a feel for messing around in 1D-Brainfuck, I highly recommend 
[Brainfuck Visualizer](https://ashupk.github.io/Brainfuck/brainfuck-visualizer-master/index.html#).

The maximum code length supported is 8092 bytes.


Demos
-----

The implementation in this project comes with a small collection of
demos.  Type any digit between `0` and `7` to see the corresponding
demo. 

[![Screenshot of Paintfuck Semi-Random Walk][IMG3]][DEMO3]

If you have an interesting demo, please
[share it under discussions][SHOW-AND-TELL]!


Canvas, Tape and Color
------

The drawing canvas is divided into a grid of 64x64 cells. Initially, all
cells of the canvas contain the value 0, which is rendered in black.

The data pointer starts in the top left corner, and can be moved using the `<`, 
`>`, `v` and `^` instructions. If the data pointer moves out of bounds, it will
wrap around, remaining in the same row for `<` and `>`, or the same column for 
`^` and `v`.

Cells can take values from 0 to 255. Overflows are simply wrapped; Applying `-`
to a cell with value `0` brings it to 255, and `+` will bring it back to `0`. 

There are a total of 8 colors. The color of any cell is determined by the cell 
value modulo 8. In order, the colors are:  

- Black
- Blue
- Green
- Cyan
- Red
- Magenta
- Yellow
- White


Code Normalisation and Validation
---------------------------------

The reference implementation performs the following two normalisation
steps (in the given order) on the input code before executing the
code:

- Lowercase letters are converted to uppercase.
- Any character in the input code that does not match a valid Paintfuck
  command is removed.

The reference implementation generates errors when the following conditions are met:

- A closing square bracket (`]`) is encountered that does not have a
  corresponding open square bracket (`[`).
- An open square bracket (`[`) is encountered while the data pointer is on a 
  `0`-valued cell, and there is no corresponding closing square bracket (`]`)

When an error is generated, the entire canvas is painted red and the
execution halts immediately.

License
-------

This is free and open source software.  You can use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of it,
under the terms of the MIT License.  See [LICENSE.md][L] for details.

This software is provided "AS IS", WITHOUT WARRANTY OF ANY KIND,
express or implied. See [LICENSE.md][L] for details.

[L]: LICENSE.md


Support
-------

To report bugs or ask questions, [create issues][ISSUES].

[ISSUES]: https://github.com/KjeldSchmidt/Paintfuck/issues
[SHOW-AND-TELL]: https://github.com/KjeldSchmidt/Paintfuck/discussions/new?category=show-and-tell

<!--
Release Checklist
-----------------

- Update version in package.json.
- Update copyright in HTML (1 place).
- Update copyright in LICENSE.md.
- Disable logging.
- Update CHANGES.md.
- Run: npm run lint
- Run: git status; git add -p
- Run: VERSION=<VERSION>
- Run: git commit -em "Set version to $VERSION"
- Run: git tag $VERSION -m "Paintfuck $VERSION"
- Run: git push origin main $VERSION
-->
