# FP

Everything follows from two things:

1. Referential Transparency (function call can be replaced by its value)
2. Composition

## RT allows

1. Local reasoning wihtout need for external context
2. Refactor without changing behavior

## Composition

enabled by **combinators**

The goal of a combinator is to create new *Thing*s from *Thing*s already defined.

## Partial functions

are a problem, because some legal arguments don't have return values.
Runtime exceptions.

## Product vs Sum type

Props {
    eidtable: boolean,
    onCHange: (string): void
}

This is a product type but should be a sum type because there is a dependency between fields
When editable=false, onChange method doesn't apply.

