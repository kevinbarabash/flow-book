# Chapter 3: Type Refinements

Before continuing read through the official docs on
[Type Refinements](https://flow.org/en/docs/lang/refinements/). It's important
to understand both what constructs will induce a type refinement as well as when
refinements will be invalidated (and how to work around it).

Now read the official [FAQ](https://flow.org/en/docs/faq/). It covers a number
of common situations that trip up beginners when using type refinement. Most of
the issues stem from not including the variable whose type we want to refine in
the type check expression.

## Beyond `if`/`else`

All of the examples in the docs use `if`/`else` statements to refine types but
other flow control statements can trigger type refinements or influence how type
refinements behave.

### Ternary

```typescript
function lengthOrNumber(a: number | string) {
    return typeof a === "number" ? a : a.length;
}
```

### `switch`

A `switch` statement functions similarly in terms of type checking to the
`if`/`else` examples from the official docs. It's a little more restrictive
since `switch` needs a value to discriminate on as opposed arbitrary logic.

```typescript
type Action =
    | {
          type: "foo";
          value: boolean;
      }
    | {
          type: "bar";
          value: number;
      };

declare var action: Action;

switch (action.type) {
    case "foo":
        (action.value: boolean);
        break;
    case "bar":
        (action.value: number);
        break;
    default:
        (action: empty);
}
```

This example also demostrates exhaustiveness checking which ensures that all
cases are handled in a `switch` statement. See
[Chapter 04: Exaustiveness Checking](04_exhaustiveness_checking.md)

### `return`

The `return` statement can influence how a type refinement behaves. Normally, a
value will loose its type refinement after exiting the `if` block.

```typescript
function foo(a: number | string) {
    if (typeof a === "string") {
        console.log(a);
    }
    (a: number); // error
}
```

If the `if` block returns though, the value will have its type refined as if it
were in an `else` block. In this case, since we return early from the function
if `a` is a `string`, Flow knows that `a` must be a `number` for the rest of the
function.

```typescript
function foo(a: number | string) {
    if (typeof a === "string") {
        return;
    }
    (a: number);
}
```

### `throw`

`throw` is very similar to the `return` example. The only thing that's different
is we're using `throw` to exit the function early.

```typescript
function foo(a: number | string) {
    if (typeof a === "string") {
        throw new Error("Oh noes, string!");
    }
    (a: number);
}
```

### `continue`

`continue` works in a similar way to `return` and `throw`, but inside of a loop.

```typescript
declare var values: Array<number | string>;

for (const a of values) {
    if (typeof a === "string") {
        continue;
    }
    (a: number);
}
```

## What doesn't work (yet?)

### Array.prototype.includes

```typescript
const fn = (val: string): "enum1" | "enum2" => {
    if (["enum1", "enum2"].includes(val)) {
        return val;
    } else {
        return "enum1";
    }
};
```

To fix this code we need to check each value in the array.

```typescript
const fn = (val: string): "enum1" | "enum2" => {
    if (val === "enum1" || val === "enum2") {
        return val;
    } else {
        return "enum1";
    }
};
```

While this isn't too bad in this example, it can get prohibitive if there are a
large number of values. This issue is being tracked in
[#6904](https://github.com/facebook/flow/issues/6904).

### Array.prototype.filter

TODO
