# Flutter Design Patterns: 11 — Abstract Factory

### Links

[Flutter Design Patterns: 11 — Abstract Factory](https://medium.com/flutter-community/flutter-design-patterns-11-abstract-factory-7098112925d8)

### Notes

##### What is the Abstract Factory design pattern?

> *Provide an interface for creating families of related or dependent objects
> without specifying their concrete classes.*

##### Analysis

- *Abstract Factory* — declares an interface of operations that create abstract *Product* objects;
- *Concrete Factory* — implements the operations to create *Concrete Product* objects. **Each Concrete Factory corresponds only to a single variant of products**;
- *Product* — declares an interface for a type of *Product* object;
- *Concrete Product* — implements the *Product* interface and defines a product object to be created by the corresponding *Concrete Factory*;
- *Client* — uses only interfaces declared by the *Abstract Factory* and *Product* classes.

Difference from Factory Method:

Abstract Factory provides a way to create a family of related objects. Factory methods is a subset of Abstract factory.

##### Implementation

Introduced a scene that can use abstract factory to create a series of widget that needs to be displayed different in different platform.

