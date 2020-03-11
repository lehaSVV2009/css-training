# CSS

[CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) Cascading Style Sheets (CSS) is a style sheet language used for describing the presentation of a document written in a markup language like HTML.

* [Animation](#animation)

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

## 3D transforms

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

### Perspective

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

## FAQ


## References

* HTML/CSS/JS course in russian - https://www.coursera.org/specializations/razrabotka-interfeysov?#courses
