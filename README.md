# CSS

[CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) Cascading Style Sheets (CSS) is a style sheet language used for describing the presentation of a document written in a markup language like HTML.

* [Display model](#display-model)
* [Position](#position)
* [Floating elements](#floating-elements)
* [Flexbox](#flexbox)
* [Animation](#animation)

# Display model

All is squares (boxes).

Square consists of:

* `content`
* `padding`
* `border`
* `margin`

Position of box is calculated by:
* Size of the box (defined, limited, not defined)
* Box type (inline, block, ...)
* Position relative to neighbours and parents/children
* Size of viewport (browser size)
* Inner proptions (images, videos with size metadata inside)
* Position scheme (static, float, absolute, ...)

## Display block

Wrap element with endlines.

```
display: block
```

Block elements:
* p
* div
* h1-h6
* ol
* ul
* form
* hr
* table
* display: block, list-item, table, flex...
* ...

Containing block - square that is used to calculate position and sizes of element/elements.

By default (if `box-sizing: content-box`)

```
containing block width = margin-left + border-left + padding-left + content-width + padding-right + border-right + margin-right 
containing block heigth != margin-top + border-top + padding-top + content-height + padding-bottom + border-bottom + margin-bottom 
```

Percent values:
* width: 10%; means 10% of containing block width
* heigth: 10; means 10% of containing block height, but only if it is explicitly defined!
* margin-left: 10%; or padding-right: 10%; means 10% of containing block width
* margin-top: 10%; or padding-bottom: 10%; means 10% of containing block WIDTH!

Custom border example:

```
div {
  border-width: 20px 40px;
  border-style: solid dashed dotted;
  border-color: blue red;
}
```

### block math

margin + width = containing block width - border - padding

Inner width is 666px:

```
.wrapper { width: 666px };
.inner { };
```

Inner width is 166px, `margin-right` is 0:

```
.wrapper { width: 666px; }
.inner { margin-left: 500px; }
```

Inner width is 66px:

```
.wrapper { width: 666px; }
.inner { margin-left: 500px; margin-right: 100px; }
```

`margin-left` is 0, `margin-right` is 566px:

```
.wrapper { width: 666px; }
.inner { width: 100px; }
```

`margin-left` is 0, `margin-right is 566px`:

```
.wrapper { width: 666px; }
.inner { width: 100px; margin-right: 0; }
```

`margin-left` is 566px, `margin-right` is 0:

```
.wrapper { width: 666px; }
.inner { width: 100px; margin-left: auto; }
```

`margin-left` is 283px, `margin-left` is 283px;

```
.wrapper { width: 666px; }
.inner { width: 100px, margin: 0 auto; }
```

Inner width is 766px, `margin-left` is 0, `margin-right` is 0:

```
.wrapper { width: 666px };
.inner { margin-right: -100px; }
```

Inner width is 866px, `margin-left` is 0, `margin-right` is 0:

```
.wrapper { width: 666px };
.inner { margin-left: -100px; margin-right: -100px; }
```

### margin collapse

Top and bottom margins of neighbour elements are sometimes collapsed into a single margin that is equal to the largest of the two margins.

This does not happen on left and right margins! Only top and bottom margins!

## Display inline

Renders as a line with no endlines.

```
display: inline
```

Specifities:

* No reactions on `height` or `width` props
* No reactions on `margin-top` or `margin-bottom`
* `padding` doesn't change containing block height

P.S. `inline-block` has no such specifities.

Block elements:
* span
* b
* i
* em
* strong
* a
* br
* img
* button
* input
* select
* label
* textarea
* display: inline, inline-block, inline-table


# Position

* static - default
* relative
* absolute
* fixed
* sticky


## static

One by one, left to right, top to bottom

```
position: static
```

## relative

Position is moved relatively to its current position. Move can be done via `top`, `left`, `right` or `bottom` properties (with positive/negative/percent values).

```
position: relative
```

In left to right text direction (`direction: ltr`) left is prioritized to right:

```
.foo {
  position: relative;
  right: -20px;
  left: -20px;
}
```

Top has always greater priority than bottom:

```
.bar {
  position: relative;
  top: 10px;
  bottom: 10px;
}
```

Percent value for `left` and `right` (`left: 50%`) is calculated from parent width.
Percent value for `top` and `bottom` (`top: 10%`) is calculated from parent height only if it is explicitly defined!

Containing block for root element (body) is a viewport.
For static and relative containing block is nearest parent.
Relative block creates new containing block inside.

## absolute

Position is moved absolutely on page via `top`, `left`, `right`, `bottom` properties.
Containing block for absolute elements is nearest parent with NON-STATIC position (or root element, if non-static not found).

```
position: absolute
```

Box on top left of page:

```
.absolute {
  position: absolute;
  top: 0;
  left: 0;
}
```

Box on top left of parent element:

```
<div class="parent"><div class="absolute">Test</div></div>

.parent {
  position: relative;
}
.absolute {
  position: absolute;
  top: 0;
  left: 0;
}
```

## fixed

Same as absolute, but positioned relatively to viewport (not changed when scroll down/up).

```
position: fixed;
```

## sticky

Same as relative, until special border. After border it works as position fixed.

```
position: sticky;
```

# Stacking context

Order of elements overlay:

1. Background and border
2. Non-static element with `z-index < 0`
3. `display: block`
4. floating elements
5. `display: inline`
6. Non-static element with `z-index: 0;` or `z-index: auto`, opacity < 1.
7. Non-static element with `z-index` > 0


Red is higher because it's non-static element with `z-index: auto`. Next is green (because last is higher) and blue is the lowest.

```
<div class="red" />
<div class="blue" />
<div class="green" />

.red {
  position: relative;
}
```

### z-index hell

Too many z-indexes..

Structure to fix:

* 1000-1999 - header
* 2000-2999 - dialog
* 3000-3999 - popup
* 4000-4999 - suggest

# Floating elements

* Element is out of static flow and moves to right/left until either parent padding or another floating element.
* Inline elements know about floating elements and wrap around them. Block elements don't react on floating elements.
* Floating elements width is determined by its content
* Floating elements margins don't collapse

```
.box {
  /* float: left | right | none */
  float: left;
}
```

`float-left` is on left side, middle text is on right side of `float-left`, `float-right` is on the right side of middle text:

```
<div>
  <div class="float-left">Left</div>
  Blabla too large text
  <div class="float-right">Right</div>
</div>

.float-left {
  float: left;
}
.float-right {
  float: right;
}
```

Put block element `box-2` right under `float-left` (force block element to react on floating element):

```
<div class="box-1">
  <div class="float-left">Left</div>
  Text
</div>
<div class="box-2">Bla</div>

.float-left {
  float: left;
}

.box-2 {
  clear: left;
}
```

Put block element `box-2` right under `float-left` or `float-right` (under the element which height is bigger):

```
<div class="box-1">
  <div class="float-left">Left</div>
  Text
  <div class="float-right">Right but it's higher than left</div>
</div>
<div class="box-2">Bla</div>

.float-left {
  float: left;
}
.float-right {
  float: right;
}

.box-2 {
  clear: both;
}
```

## Block formatting context

It is a page area, where floating elements interact with each other.

It can be created by:
* float elements
* position: absolute | fixed
* display: inline-block | table-cell | table-captions (non-block elements)
* block elements with overflow: hidden | auto

`wrapper1` size depends on floating elements size inside:

```
.wrapper1 {
  overflow: hidden;
}

<div class="wrapper1">
  <div class="float-left"></div>
  <div class="float-right"></div>
</div>
```

`wrapper2` avoids floating elements outside:

```
.wrapper2 {
  overflow: hidden;
}

<div class="wrapper1">
  <div class="float-left" />
  <div class="float-right" />
</div>
<div class="wrapper2" />
```

Locate elements horizontally and reverse their order:

```
<div class="box1" />
<div class="box2" />
<div class="box3" />
<div class="box4" />

.box1, .box2, .box3, .box4 {
  float: right;
}
```

3-layer layout, where center element width is changed with resizing, but left and right - not:

```
<div class="left" />
<div class="center" />
<div class="right" />

.left { float: left; }
.right { float: right; }
.center { overflow: hidden; }
```

Clearfix - force block element to react on inner float element without `overflow: hidden`:

```
<div class="box">
  <div class="float-left" />
</div>

.float-left {
  float: left;
  height: 100px;
}
.box:after {
  content: '';
  display: block;
  clear: both;
}
```

# Flexbox

A long time ago layouting was done by `table`. 

Then - `css` and `float`/`inline-block`/`position`/... 

But solution had following problems:
* hacks like `clearfix`
* `absolute` position takes element out of the document flow
* `display: table` - too many css properties
* spaces in `inline-block`

Then/now - `flexbox`. 



Now/in future - `grid`.

# Animation

## Transforms

* translate
* scale
* rotate
* skew

### translate

Moves element on absolute/relative distance

```
transform: translate(100px, 100px); // (X, Y);
transform: translate(-300px, -100px); // move to 300px left and 100px up;
transform: translate(100%, -100%); // move to full width right and full height up;
transform: translate(0, 50%); // move to 50% height bottom;

transform: translateX(100%);
transform: translateY(-50%);
```

### scale

Increase/decrease size of element. (Only on UI, not in html)

```
transform: scale(1.5); // increase;
transform: scale(0.5); // decrease;
transform: scaleX(0.5);
transform: scaleY(1.5);

transform: scale(1, -1); // turn over from top to bottom;
transform: scale(-1, 1); // turn over from left to right;
transform: sclae(-2); // turn over from top to bottom and from left to right;
```

### rotate

```
tranform: rotate(180deg); // clockwise;
trasform: rotate(-180deg); // counterclockwise;
trasform: rotate(1.5turn);
```

### skew

```
transform: skew(0, 45deg); // like italic;
```

### matrix

Usually used with JS.

```
matrix(a, b, c, d, tx, ty)
a - scaleX
b - skewX
c - skewY
d - scaleY
tx - translateX
ty - translateY
```

```
.matrix {
  tranform: matrix(2, 0, 0, 1, 0, 0);
}
```

## Combining transforms

Scale element + move + rotate

```
.box {
  transform: scale(2) translateX(100px) scale(2);
}
```

Rotate element + move + scale:

```
.box {
  transform: rotate(45deg) translateX(100px) scale(2);
}
```

Coordinate system is changed after every transformation (3 times in previous example).

### transform-origin

```
transform-origin: 0; // left top point;
transform-origin: right bottom; // right bottom point;
transform-origin: 50%; // center point - default;
transform-origin: 50px; // 50px from left and 50px from top point;
transform-origin: 50% 100%; // center bottom point;
```

e.g. rotate element using top-left point as a center:

```
.box {
  transform: rotate(45deg);
  transform-origin: top left;
}
```

## Transitions

Transition - an animation from one set of css properties to another.

Arguments:
1. Initial set of CSS properties
2. Final set of CSS properties
3. Transition type with characteristics
4. Action that initiates change (`:hover`, `:target`, `:focus`, `:active`, JS, new class/id)

Simple animated button scale on hover:

```
.button {
  transition-property: transform;
  transition-duration: .3s;
}

.button:hover {
  transform: scale(1.2);
}
```

Simple animated button scale and background change on hover:

```
.button {
  transition-property: transform, background-color;
  transition-duration: .3s, 300ms;
  background-color: #ccc;
}
.button:hover {
  transform: scale(1.2);
  background-color: #f00;
}
```

Simple animated button scale and background change (after 0.5s) on hover:

```
.button {
  transition-property: transform, background-color;
  transition-duration: 0.3s, 500ms;
  transition-delay: 0s, 0.5s;
  background-color: #ccc;
}

.button:hover {
  transform: scale(1.2);
  background-color: #f00;
}
```

`transition-timing-function`:
* `linear` - normal at the start, normal in the middle, normal at the end
* `ease`
* `ease-in` # slow at the start, fast at the end
* `ease-out` # fast at the start, slow at the end
* `ease-in-out` # fast at the start, normal in the middle, slow at the end
* `step-start` - at once at the end
* `step-end` - all the time at the start
* `steps(10)` - separate time to 10 steps
* `steps(10, start)` - separate time to 10 steps with changed start point
* `cubic-bezier` - custom

Flying rocket:
```
.rocket {
  transition-property: transform;
  transition-timing-function : cubic-bezier(.98, 0, 1, .28); // x1, y1, x2, y2;
  transition-duration: 3s;
}
```

Transition shorthand:
```
.long {
  transition-property: transform;
  transition-duration: .5s;
  transition-timing-function: ease-in;
  transition-delay: 1s;
}

.short {
  transition: transform .5s ease-in 1s;
}

.multi-short {
  transition: transform .5s ease-in,
              background-color .5s ease-in 1s;
}

.super-short {
  transition: all .5s;
}

```

## Keyframes

When transition and transform are not enough keyframes can help. They allow to create your own animations.

```
@keyframes animationName {
  from {
    /* css properties of the 1st frame */
  }
  to {
    /* css properties of the 2nd frame */
  }
}
```

Simple example:

```
.box {
  opacity: 0;
}
.box.visible {
  animation-name: show;
  animation-duration: 2s;
}

@keyframes show {
  to {
    opacity: 1;
  }
}
```

*It makes sense to has property same as property in `to`*

Blink example:

```
.box:hover {
  animation-name: blink;
  animation-duration: 2s;
}
@keyframes blink {
  0% {  // 0% is the same as from;
    background-color: blue;
  }
  50% {
    background-color: red;
  }
  100% { // 100% is the same as to;
    background-color: green;
  }
}
```

Ease in-out with delay:

```
.box:hover {
  animation-name: show;
  animation-duration: 4s;
  animation-delay: 1s;
}
@keyframes blink {
  0% {
    opacity: 0;
    background-color: blue;
  }
  25%, 75% {
    background-color: green;
  }
  100% {
    opacity: 1;
    background-color: grey;
  }
}
```

Change timing function in animation:

```
.box.move {
  animation-name: move;
  animation-duration: 8s;
  animation-timing-function: ease-in;
}

@keyframes move {
  0% {
    transform: translate(0, 0);
  }
  25% {
    transform: translate(100%, 0);
    animation-timing-function: linear;
  }
  50% {
    transform: translate(100%; 200%);
  }
  75% {
    transform: translate(0, 200%);
    animation-timing-function: linear;
  }
  100% {
    transform: translate(0, 0);
  }
}
```

```
animation-iteration-count: 3; // animation will be repeated 3 times;
```

Clocks animation:

```
.seconds {
  animation-name: seconds;
  animation-duration: 60s;
  animation-iteration-count: infinite;
}
```

Fix jumps

```
animation-direction: alternate;
animation-fill-mode: forwards;
```

Shorthands

```
.long {
  animation-name: scale;
  animation-duration: 2s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: 3;
  animation-direction: alternate;
  animation-delay: 5s;
  animation-fill-mode: forwards;
}
.short {
  animation: scale 2s ease-in-out 3 alternate 5s forwards;
}
.multi-short {
  animation: scale 2s linear, move 2s ease-out;
}
```

Pause animation

```
.heart {
  animation: heartBeat 1s ease infinite;
}
.heart:hover {
  animation-play-state: paused;
}
```

## Animation Examples

### Print `css is awesome` with caret animation

```
<div class="typing">css is awesome</div>

document.getElementsByClassName('typing')[0].classList.add('visible');

.typing {
  width: 0;
  white-space: nowrap;
  overflow: hidden;
  border-right: 1px solid;
}
.typing.visible {
  animation: caret 1s steps(1) infinite,
             typing 4s steps(15) forwards;
}
@keyframes caret { 50% { border-color: transparent; } }
@keyframes typing { to { width: 15ch; } }
```

### Spin circle

Small circle is inside big circle, on top of it. Rotate big circle without rotating small circle inside.

1. Locate image in the center of circle
2. Rotate small circle (it changes coordinate system) -> move small circle to large circle border with translate -> rotate small circle back.

```
<div class="circle"><img src="cat"></div>

.circle.run img {
  animation: spin 4s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0turn)
               translateY(-150px)
               rotate(1turn)
  }
  100% {
    transform: rotate(1turn)
               translateY(-150px)
               rotate(0turn)
  }
}
```

### block !important

```
<div class="box box-overwrite" style="background: #fff !important;"></div>

.box-overwrite {
  animation: break-style 1s infinite;
}
@keyframes break-style {
  from { background: red }
  to { background: red }
}
```

## 3D

### transforms

Required for 3d
```
transform-style: preserve-3d # flat by default;
```

```
rotateX(360deg);
rotateY(360deg);
rotateZ(360deg);
rotate3d(1, 1, 1, 180deg); // x, y, z, angle;

translateX(100%)
translateY(200px);
translateZ(100%)
translate3d(10px, 100%, 20px);

scale(2);
scale3d(1, 1.5, 2);

matrix3d(a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p)
```

### perspective

Perspective - distance from user's eye to object surface

```
perspective: none; // object seems flat;
perspective: 200; // object is far;
perspective: 400; // object is too far;
```

Perspective origin is point where user's eye is potentially located

```
perspective-origin: 50% 50%; // default;
perspective-origin: 0 0; // eye seems to be on left top point;
perspective-origin: 50% 0; // eye seems to be on center top point;
```

### backface-visibility

Defines visibility of back side in 3d object.

```
backface-visibility: visible;
backface-visibility: hidden;
```


## FAQ


## References

* HTML/CSS/JS course in russian - https://www.coursera.org/specializations/razrabotka-interfeysov?#courses
