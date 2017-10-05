---
published: false
---
## Concrete vs Abstract vs Interface

Concrete
With inheretance in mind, concrete classes have several disadvantages and gotchas that makes them less appelind candidats to derive from. If, for example, method is not abstract, nothig forces child class to implement it. If this is unintentional, it could lead to runtime error.

Abstract
Abstract classes on the other side contain abstract members, forcing child classes to implement these members, thus enforing functionalities. Also, we can make use of access modifiers: public, private, proteced and internal, to fine tune what we want to expose.
Asstract classes can contain everything we can put in normal class (fields, propertes, constructors, destructiors, methods, events and indexers).
The main limitation with class inheretance is that it is only possible to derive from a single class. 

Interface
Interfaces are best described as contracts, the list of members we expect to be implemented by some class. Class implementing the interface are obligated to implement all its members. All these members of the class are automaticly public.
You can implenent multiple interfaces (sagregation)
All members are automatically public (contract terms)
Can contain only Properties, Methods, Events and Indexers
