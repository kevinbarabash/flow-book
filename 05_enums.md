# Chapter 5: Enums

In JavaScript it's common to store enums in variables or a dictionary. This
takes some extra work to get things right with Flow.

```typescript
const Suits = {
    Diamonds: "Diamonds",
    Clubs: "Clubs",
    Hearts: "Hearts",
    Spades: "Spades"
};

type Suit = $Values<typeof Suits>;

const callTrump = (suit: Suit) => {
    // set the trump suit
};

callTrump(Suits.Diamonds);
callTrump("Rubies"); // no error, but there should be
```

The problem is, is that each of the values in `Suits` is typed as a `string` so
the type returned by `$Values<typeof Suits>` is `string`.

There are a couple of possible fixes for this.

```typescript
const Suits = {
    Diamonds: ("Diamonds": "Diamonds"),
    Clubs: ("Clubs": "Clubs"),
    Hearts: ("Hearts": "Hearts"),
    Spades: ("Spades": "Spades"),
};

type Suit = $Values<typeof Suits>;

const callTrump = (suit: Suit) => {
    // set the trump suit
}

callTrump(Suits.Diamonds);
callTrump("Rubies"); // error, "Rubies" is incompatible with enum Suits
```

That's a lot of typing though. Using union types though is a lot simpler.

```typescript
type Suit = "Diamonds" | "Clubs" | "Hearts" | "Spades";

const callTrump = (suit: Suit) => {
    // set the trump suit
};

callTrump("Diamonds");
callTrump("Rubies"); // error, "Rubies" is incompatible with enum Suits
```

When using union types though, you have to use the actual values in the code
which means that:

-   no support for auto-complete yet
-   harder to minify (maybe gzip handles this)
