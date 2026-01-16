---
title: "Core ODD principles to constantly revise"
date: 2026-01-16
featured: false
draft: false
toc: true
categories:
  - coding
tags:
  - python
  - ODD
  - coding
series:
  - diary-of-a-flawed-pythonista
---

## Summary

Grouping together core concepts of Object-Oriented Design for programming. This also server the purpose of a refresher
ahead of job interviews.

## Good ODD

A good design ensures code longevity. Consider the following:

* Add behavior instead of rewriting existing code.
* Keep systems testable, readable and explainable.

## Single responsibility principle

> a class or module should have only one reason to change, meaning it should have one single, well-defined
> responsibility, grouping together code that changes for the same reasons and separating it from code that changes for
> different reasons

## Open/Closed principle

> Open for Extension, closed for modification

## Liskov substitution Principle

If a subclass breaks expectations:

* Throws new exceptions
* Changes behavior semantics
* Requires special handling

Then, it violates LSP and becomes a bug magnet.

See example of a `Penguin` extending `Bird`, and not being able to fly(). It drove the point home.
See Medium
post: [Liskov Substitution Principle](https://medium.com/code-and-concepts/part-4-liskov-substitution-principle-lsp-1f4634ce93fa)
for a more detailed explanation

## Interface Segregation Principle

Prefer many small interfaces over one large one. Personal broader approach applies here, break big problem into smaller
chunks.

> The Interface Segregation Principle (ISP) states that no client should be forced to depend on methods it does not use.

- [(Kumar, 2024)](https://medium.com/code-and-concepts/part-5-interface-segregation-principle-isp-337c7862c2dc)

### Segway to definition of an Interface

Interface
An interface defines a contract that specifies what methods a class must implement, without providing the method
implementations themselves. It is used to enforce consistent behavior across different classes, promote loose coupling,
and enable polymorphism by allowing different implementations to be used interchangeably.

## Dependency Inversion Principle

> Depend on abstractions, not implementations

According to DIP, high-level modules should not depend on low-level modules directly. Instead, both should depend on
abstractions (interfaces or abstract classes).

## References

1. Martin, R.C., 2017. Clean architecture: a craftsman's guide to software structure and design. Prentice Hall Press.
2. Kumar, B. (2024). Part 4: Liskov Substitution Principle (LSP). [online] Medium. Available
   at: https://medium.com/code-and-concepts/part-4-liskov-substitution-principle-lsp-1f4634ce93fa [Accessed 16 Jan. 2026].