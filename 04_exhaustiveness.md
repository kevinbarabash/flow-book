# Chapter 4: Exhaustiveness Checking

Flow can perform exhaustivness checking on `switch` statements by casting the
variable being refined by the `switch` to `empty` like so:

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
