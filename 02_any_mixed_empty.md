# Chapter 2: `any`, `mixed`, and `empty`

`any`

### `mixed` vs `empty`

A value of any type (not be confused with the `any` type) can be assigned to a
value of `mixed` type.

```typescript
declare var a: number;
declare var b: boolean;
declare var c: string;

let m: mixed;

m = a;
m = b;
m = c;
```

`empty` on the other hand is a type to which no value can be assigned. It can be
helpful to think of this type as being the empty set. There are no values in the
empty set.

```typescript
declare var a: number;
declare var b: boolean;
declare var c: string;

let m: mixed;

m = a; // error
m = b; // error
m = c; // error
```

### `mixed` vs `any`

When you don't know the type of a value you can use either `mixed` or `any` as
its type. If you still want your code to be type-safe you should use `mixed`
though since `any` opts out of type checking completely.

```typescript
declare unsafeObj: any = JSON.parse("...");
(unsafeObj.foo: string); // no error

const safeObj: mixed = JSON.parse("...");
(safeObj.foo: string); // error
```

The problem with `unsafeObj` is that it's possible that the string being parsed
doesn't contain an object with a `foo` property that's a string. But how do we
go about use `safeObj` without Flow raising an error? We need to add appropriate
type checks.

```typescript
const safeObj: mixed = JSON.parse("...");
if (safeObj && typeof safeObj.foo === "string") {
    (safeObj.foo: string); // no error
}
```
