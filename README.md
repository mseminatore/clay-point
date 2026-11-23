# point

2D points and vectors for Clay: arithmetic, length, distance, dot and cross products.

A `Point` is a location; a `Vector` is a displacement (a length and a direction, with no
position). They are separate types, and the arithmetic between them follows the usual rules:

| Expression         | Result                                   |
|--------------------|------------------------------------------|
| `Point - Point`    | `Vector`                                 |
| `Point + Vector`   | `Point`                                  |
| `Point - Vector`   | `Point`                                  |
| `Vector ± Vector`  | `Vector`                                 |
| `Vector * number`  | `Vector`                                 |
| `Point + Point`    | raises (adding two locations is meaningless) |

## Usage

```clay
import("point")

a = Point new(3, 4)
b = Point new(1, 2)
d = a - b              // Vector(2, 2)
b + d                  // Point(3, 4)
```

`Point new(x, y)` and `Vector new(x, y)` take two numbers (integers or floats) and raise
otherwise.

## Point

```clay
a distance(b)                   // 2.828427
a distanceSquared(b)            // 8
a lerp(b, 0.5)                  // Point(2, 3)
a midpoint(b)                   // Point(2, 3)
a orientation(b, Point new(0, 5))   // z of (b - a) x (c - a): > 0 counterclockwise, < 0 clockwise, 0 in a line
a toVector                      // Vector(3, 4): the offset from the origin
```

`setX` and `setY` change the point itself and return it, so calls chain:

```clay
p = Point new(0, 0) setX(1) setY(2)   // Point(1, 2)
```

`x` and `y` are also plain slots (`p x`, `p y = 5`), and `copy` makes an independent point.
`distance`, `distanceSquared` and `lerp` raise if given something other than a point.

## Vector

The operators return new vectors and leave their operands unchanged:

```clay
u = Vector new(3, 4)
v = Vector new(1, 2)

u + v              // Vector(4, 6)
u - v              // Vector(2, 2)
u * 2              // Vector(6, 8)
u / 2              // Vector(1, 2) — integer vectors divide like integers
-u                 // Vector(-3, -4) (also u negated)
```

```clay
u length                       // 5
u lenSquared                   // 25
u dot(v)                       // 11
Vector new(1, 0) cross(Vector new(0, 1))   // 1 (> 0: counterclockwise)
u normalized                   // Vector(0.6, 0.8); raises for a zero-length vector
u perpendicular                // Vector(-4, 3): 90° counterclockwise
u rotate(angle)                // rotated counterclockwise by angle radians
u angleTo(v)                   // signed angle from u to v
u reflect(normal)              // reflected off a surface with this unit normal
u project(onto)                // the part of u along another vector
u lerp(v, 0.5)                 // halfway between u and v
Vector fromAngle(angle)        // unit vector at an angle in radians
u toAngle                      // angle from the +x axis
u toPoint                      // Point(3, 4)
```

Vectors have no setters; a method never changes a vector in place.

## Equality

`==` compares coordinates, and `!=` is its opposite. A point or vector is never equal to
something of another type (`nil`, a number, a string), and a `Point` never equals a `Vector`
even with the same coordinates.

```clay
Point new(1, 2) == Point new(1, 2)   // true
Point new(1, 2) == Vector new(1, 2)  // false
Point new(1, 2) == nil               // false
```

`toString` gives `Point(3, 4)` or `Vector(3, 4)`, which string interpolation uses too.

## Upgrading from 0.2

Version 0.3 splits the old `Point`. Vector math (`length`, `dot`, `cross`, `normalized`,
`rotate`, `perpendicular`, `reflect`, `angleTo`, `fromAngle`, `toAngle`, `*`, `/`, `negated`,
`scale`) moved to `Vector`, and `Point + Point` now raises. Replace `a + b` with
`a + b toVector`, and take lengths and dot products on `a - b` or `a toVector`.
`Point scale` and the setters on vectors were dropped; use `*` instead.

## Tests

```sh
clay tests/test_point.clay
clay tests/test_vector.clay
```
