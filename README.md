# AI4P
An inference that always brings peace

### Problem Statement

There are features in programming languages that are desirable by themselves but also run at odds to one another. I have 12 such features and a language that contains all of them but the type inference algorithm is not obvious.

I suspect that strong normalization is possible in this language, but it remains an open problem. When type inference is complete we can say that the program is "at peace" so that is where this project name comes from.

### Language Specification

[Term]
A Term can always be represented as a lambda calculus term.

[Type]
A Type can always be represented nominally with possible internal structure expressable as a lamba calculus term.

[Split]
A Split is a symbolic link to multiple ambiguous terms.
Any Term position that accepts a Term must accept a Split.

[Join]
A Join is a symbolic link to multiple ambiguous types.
Any Type position that accepts a Type must accept a Join.

[Goal]
A Goal is an objective to minimize or maximize a metric produced from some Terms and Types.

[Relation]
A relation is a property of comparison of some Terms and Types.
A relation is considered a Boolean Term even though it may reference both Terms AND Types.
Any Term position that accepts a Boolean Term must accept a Relation.

### Example
