# AI4P
An inference that always brings peace

### Problem Statement

There are features in programming languages that are desirable by themselves but also run at odds to one another. I have 12 such features and a language that contains all of them but the type inference algorithm is not obvious.

I suspect that strong normalization is possible in this language, but it remains an open problem. When type inference is complete we can say that the program is "at peace" so that is where this project name comes from.

### Language Specification

[term] A term can always be represented as a lambda calculus term.

[type] A type can always be represented nominally with possible internal structure expressable as a lamba calculus term.

[split] A split is a symbolic link to multiple ambiguous terms. Any term position that accepts a Term must accept a Split.

[join] A join is a symbolic link to multiple ambiguous types. Any type position that accepts a Type must accept a Join.

[goal] A goal is an objective to minimize or maximize a metric produced from some Terms and Types.

[relation] A relation is a property of comparison of some Terms and Types.
