# Flutter Design Patterns: 10 — Factory Method

### Links

[Flutter Design Patterns: 10 — Factory Method](https://medium.com/flutter-community/flutter-design-patterns-10-factory-method-c53ad11d863f)

### Notes

##### What is the Factory Method design pattern?

> *Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to sub­classes.*

##### Analysis

- Creator--abstract class, declares factory method.
- ConcreteCreator-- overrides the factory method.
- Product-- defines a common interface to all objects
- ConcreteProduct-- implements the product interface

##### Implementation

Introduced a scene that can use Factory method design pattern where a dialog needs to be shown different in different platform.