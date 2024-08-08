Object-oriented programming (OOP) is a way of modelling programs.

##### Characteristics of Object-Oriented Languages

There is no consensus on ewhat features are required to make it be OOP, or any other paradigm, but there are certain common characteristics, namely objects, [[encapsulation]], and [[inheritance]] that keep appearing.

According to The Gang of Four (Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides) who wrote the Bible of OOP, _Design Patterns: Elements of Reusable Object-Oriented Software_ defined it in this way:

>Object-oriented programs are made up of objects. An _object_ packages both data and the procedures that operate on that data. The procedures are typically called _methods_ or _operations_.

With this definition Rust is object-oriented. Structs and enums have data and `impl` blocks provide methods for those structs and enums. They aren't called objects but provide the same functionality.

##### Encapsulation that Hides Implementation Details

Another aspect of OOP is _encapsulation_, which means that the implementation details of an object are NOT accessible to code using that object. Therefore the only way to interact with an object id through its public API