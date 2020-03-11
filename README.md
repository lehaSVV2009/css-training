# CSS

[CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) Cascading Style Sheets (CSS) is a style sheet language used for describing the presentation of a document written in a markup language like HTML.

* [Animation](#animation)

## Animation

### translate

```
transform: translate(100px, 100px); // (X, Y)
transform: translate(-300px, -100px); // move to 300px left and 100px up
transform: translate(100%, -100%); // move to full width right and full height up
transform: translate(0, 50%); // move to 50% height bottom

transform: translateX(100%);
transform: translateY(-50%);
```

### scale

```
transform: scale(1.5); // increase
transform: scale(0.5); // decrease
transform: scaleX(0.5);
transform: scaleY(1.5);

transform: scale(1, -1); // turn over from top to bottom
transform: scale(-1, 1); // turn over from left to right
transform: sclae(-2); // turn over from top to bottom and from left to right
```

### rotate

```
tranform: rotate(180deg); // clockwise
trasform: rotate(-180deg); // counterclockwise
trasform: rotate(1.5turn);
```

### skew

```
transform: skew(0, 45deg); // like italic
```

## FAQ


## References

* HTML/CSS/JS course in russian - https://www.coursera.org/specializations/razrabotka-interfeysov?#courses
