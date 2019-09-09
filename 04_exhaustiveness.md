# Chapter 4: Exhaustiveness Checking

Exhaustiveness checking is the ability for a type checker to make sure that
we've handled all possible types that value may be when doing a type check. Flow
can do this with `switch` like so:

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

The key part of this is the `(action: empty)` cast in the `defualt` clause.
Normally casting a variable to `empty` will cause a Flow error. In this case it
doesn't because Flow has determined that the `default` clause is unreachable.
Flow doesn't type check code that's unreachable.

To check that this does what we want, let's remove one of the `case` clauses:

```typescript
switch (action.type) {
    case "foo":
        (action.value: boolean);
        break;
    default:
        (action: empty); // error, cannot cast action to empty
}
```

Now that there's a case that isn't being handled, the `default` clause is not
longer unreachable so Flow type checks the code in that clause which triggers an
error.
