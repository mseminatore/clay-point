# stdlib/point

2D points and vectors for Clay: arithmetic, length, distance, dot and cross products.

## Usage

```clay
import("stdlib/point")

a = Point new(3, 4)
b = Point new(1, 2)
```

`Point new(x, y)` takes two numbers (integers or floats) and raises otherwise.

## Arithmetic

The operators return new points and leave their operands unchanged:

```clay
a + b              // Point(4, 6)
a - b              // Point(2, 2)
a * 2              // Point(6, 8)
a / 2              // Point(1, 2) — integer points divide like integers
-a                 // Point(-3, -4) (also a negated)
```

`setX`, `setY` and `scale(factor)` change the point itself and return it, so calls chain:

```clay
p = Point new(0, 0) setX(1) setY(2)   // Point(1, 2)
p scale(3)                             // p is now Point(3, 6)
```

`x` and `y` are also plain slots (`p x`, `p y = 5`), and `copy` makes an independent point.

## Vector math

```clay
a length                       // 5
a lenSquared                   // 25
a distance(b)                  // 2.828427
a dot(b)                       // 11
Point new(1, 0) cross(Point new(0, 1))   // 1 (> 0: counterclockwise)
a normalized                   // Point(0.6, 0.8); raises for a zero-length point
Point new(0, 0) lerp(Point new(10, 20), 0.5)   // Point(5, 10)
```

## Equality

`==` compares coordinates, and `!=` is its opposite. A point is never equal to something
that isn't a point (`nil`, a number, a string).

```clay
Point new(1, 2) == Point new(1, 2)   // true
Point new(1, 2) == nil               // false
```

`toString` gives `Point(3, 4)`, which string interpolation uses too.

## Tests

```sh
clay lib/stdlib/point/tests/test_point.clay
```
