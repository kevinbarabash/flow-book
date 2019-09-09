# Chapter 7: Standard Library Gotchas

The way some functions in the standard library may be a bit surprising. The goal
of this library is to both highlight those functions but also explain why
they're typed the way they are.

## Object.values

The current definition of `Object.values` is:

```typescript
static values(object: $NotNullOrVoid): Array<mixed>;
```

It might seem plausible to define `Object.values` in the following way:

```typescript
static values<K: string, V>(object: {[K]: V}): Array<V>;
```

But then we'd run into the following situation:

```typescript
type Point = { x: number; y: number };

const pointWithId: Point = { id: "center", x: 0, y: 0 };

const values: Array<number> = Object.values(pointWithId);
```

This is unsafe since `values[0]` is actually a string.

We could improve upon this by requiring the type of the object to be exact. This
would work in some situations, but not all. In particular it wouldn't handle
types such as `{[string]: number}`. This type isn't exact but it does make sense
for the type of the return value of `Object.values` to be `Array<number>` in
this case. Flow doesn't have a way to model this distinction yet. See
[#6927](https://github.com/facebook/flow/issues/6927) for more details.

## Array.prototype.map

TODO

## Array.prototype.filter

TODO

## document.body

TODO
