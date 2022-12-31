Table of contents:
- What is Singleton Pattern?
- What is the motivation of Singleton Pattern?
- Implementation of Singleton Pattern?

## What is Singleton Pattern?

A component which is instantiated only once.

The Singleton pattern is a design pattern that ensures that a class has only one instance and provides a global access point to it. The purpose of the Singleton pattern is to control object creation, limiting the number of objects to only one. This is useful when exactly one object is needed to coordinate actions across the system.

## What is the motivation of Singleton Pattern?

- For some components it only makes sense to have one system

    - Database repository
    - Object factory
- E.g. Constructor call is expensive
    - We only do it once
    - We provide everyone with the same instance
- Want to prevent anyone creating additional copies
- Need to take care of lazy instantiation and thread safety

## Implementation of Singleton Pattern