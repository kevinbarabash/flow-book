# Chapter 2: `any`, `mixed`, and `empty`

From the official docs:

> If you want a way to opt-out of using the type checker, `any` is the way to do
> it. **Using any is completely unsafe, and should be avoided whenever
> possible.**

## `mixed` vs `any`

When you don't know the type of a value you can use either `mixed` or `any` as
its type. If you still want your code to be type-safe you should use `mixed`
though since `any` opts out of type checking completely.

```typescript
declare var unsafeObj: any;
(unsafeObj.foo: string); // no error, unsafe

declare var safeObj: mixed;
(safeObj.foo: string); // error, foo is missing in mixed
```

The problem with `unsafeObj` is that it's possible that it does not contain an
`foo` property that's a `string`. Since we've opted out of type checking by
using `any` though Flow won't tell us there's an issue. If we use `mixed` to
describe a value whose type is unknown Flow will alert us to the issue.

But how do we go about use `safeObj` without Flow raising an error? We need to
add an appropriate type check. In this case we need to make sure that `safeObj`
is defined and that it has a `foo` property of the correct type.

```typescript
declare var safeObj: mixed;
if (safeObj && typeof safeObj.foo === "string") {
    (safeObj.foo: string); // no error, safe
}
```

## `mixed` vs `empty`

Any value can be assigned to a variable of `mixed` type. Because of this,
`mixed` can be described as the
[top type](https://en.wikipedia.org/wiki/Top_type) in Flow. All other types are
subtypes of `mixed`.

```typescript
declare var a: number;
declare var b: string;

(a: mixed);
(b: mixed);
```

`empty` on the other hand is a type to which no value can be assigned. It can be
helpful to think of this type as being the empty set. There are no values which
this type describes. It may also be referred to as the
[bottom type](https://en.wikipedia.org/wiki/Bottom_type). Is is a subtype of all
other possible types in Flow.

```typescript
declare var a: number;
declare var b: string;

(a: empty); // error
(b: empty); // error
```
