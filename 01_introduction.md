# Chapter 1: Introduction

The purpose of this book is to provide more details and fill in some of the gaps
in the official Flow documentation. It is not meant to repleace existing
documentation. The existing docs are pretty good and this book references them
frequently. There are however some gaps in the material both for beginners as
well as for some more advanced features. This book does not pretend to fill all
of those gaps either. The hope though is that it will fill more of them as time
goes on.

## Effective Flow

Flow is a static type checker for JavaScript code. It does not change the
semantics of JavaScript. That being said, there are certain constructs in
JavaScript that it doesn't understand. It can't provide guarantees about the
type safety of these constructs. Flow errs on the side of type safety when it
doesn't understand something and thus Flow will sometimes complain about code
which you as a developer know to be right.

To effectively use Flow we need to know which constructs it understands and
avoid those which it doesn't in our code.

Throughout this book we'll use Flow's type casts to make assertions about types.
Many of the examples will also make use of the Flow's `declare` syntax to
declare the existence of variables of certain types in the example. This is
because we don't care about the values of the variables. We assume that there's
some code elsewhere that is produces a variable of the declared type.

## Learning Flow

When in doubt about the behavior of a certain construct it can be helpful to
validate the behavior for oneself using a simple example. The flow playground
can come in quite handy for this. Give it a try at https://flow.org/try/. The
playground also provides permalinks for any code examples you've typed in which
is helpful when sharing examples with others.
