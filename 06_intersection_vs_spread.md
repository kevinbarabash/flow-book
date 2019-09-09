# Chapter 6: Intersection Types vs Object Type Spread

Intersection types are commonly used to combine object types. There are
unfortunately some gotchas when doing this. In particular it can lead to
properties in the object being unassignable because the intersection of the
properties is impossible.

Example from
[Intersection Types](https://flow.org/en/docs/types/intersections/):

```typescript
// @flow
type One = { prop: number };
type Two = { prop: boolean };

type Both = One & Two;

var value: Both = {
    prop: 1 // error, number is incompatible with prop
};
```

Example from [Object types](https://flow.org/en/docs/types/objects/):

```typescript
// @flow

type FooT = {| foo: string |};
type BarT = {| bar: number |};

type FooBarFailT = FooT & BarT;
type FooBarT = {| ...FooT, ...BarT |};

const fooBarFail: FooBarFailT = { foo: '123', bar: 12 }; // Error!
const fooBar: FooBarT = { foo: '123', bar: 12 }; // Works!
```

Given how easy it is to use intersection types incorrectly, it's preferable to
use object type spread when combining types. It's more predictable. If there are
duplicate keys than the key wins.
