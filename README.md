# AI4P
An inference that always brings peace

### Problem Statement

There are features in programming languages that are desirable by themselves but also run at odds to one another. I have 6 such features and a language that contains all of them but the type inference algorithm is not obvious.

I suspect that strong normalization is possible in this language, but it remains an open problem. When type inference is complete we can say that the program is "at peace" so that is where this project name comes from.

### Language Specification

[Term]
A Term can always be represented as a lambda calculus term.

[Type]
A Type can always be represented nominally with possible internal structure expressable as a lambda calculus term.
Types can be viewed as ordinals or literal strings etc, so these Types are equivalent to types in Simply Typed Lambda Calculus.

[Split]
A Split is a symbolic link to multiple ambiguous Terms.
Any Term position that accepts a Term must accept a Split.

[Join]
A Join is a symbolic link to multiple ambiguous Types.
Any Type position that accepts a Type must accept a Join.

[Projection]
A Projection is a lambda calculus term that can be applied to a Type structure to return another Type.
A Projection must be strongly normalizing.

[Relation]
A relation is a property of comparison of some Terms and Types.
A relation is considered a Boolean Term even though it may reference both Terms AND Types.
Any Term position that accepts a Boolean Term must accept a Relation.

### Constraints

There are some constraints on what constitutes a well-formed language program:
* Each individual Term in a Split is explicitly ascripted with a Type.
* Join Reduction (is only meant to add ad-hoc polymorphism, but there may be other reductions)

There may be additional constraints added to reach strong normalization, but for now I am just considering the shape of the most general problem.

### Example

There have been several attempts so far to implement languages with the above feature set.
Type inference in these languages does seem to be strongly normalizing, but in practice the inference is often intractable.
The following algorithm has been discovered as effective to bring this inference in line with expectations for runtime efficiency.

<img src="https://raw.githubusercontent.com/andrew-johnson-4/AI4P/refs/heads/main/3bfc1198-5ee5-44ce-a252-518befdd4fda_700x700.webp">

To read this diagram, the left and the right are both state machines, having a "position" at one of the nodes at any given time.
To use this during inference the following information is tracked:

* Current location (t) + embedding
* Previous location (t-1) + embedding

Then the inference tries to predict the best embedding to move towards.
An embedding is a single step of natural induction.
Each color is assigned by the inference algorithm and can be used to explore a high-dimensional space.
Depending on the algorithm used, inference may fail, and back-tracking can be used to reground the state.

Here is an embedding from one particular inference algorithm.

| name       | descriptive name             | definition
|------------|------------------------------|--------------------------
| black circle     | term                             | a
| black	line       | term                             | a(b)
| white circle     | type                             | B
| white	line     | type                               | A->B
| yellow circle    | join                             |	let a: A; let b: A
| yellow line    | join                               |	let f: A->C; let f: B->C
| orange circle    | split                            |	let a: A; let a: B
| orange line    | split                              |	let f: A->B; let f: A->C
| red circle       | less                             |	≤
| red line       | less                               |	≤
| blue circle      | more                             |	≥
| blue line	     | more                               | ≥
| purple circle    | up                               |	+
| purple line    | up                                 |	+
| green	circle     | down                             |	-
| green	line     | down                               |	-
| pink circle     | parallel                          |	\|\|
| pink line	     | parallel                           |	\|\|
| grey circle	     | perpendicular                    |	⊥
| grey line	     | perpendicular                      |	⊥

This embedding only uses 10 colors. So the state machine is customizable, although there tends to be overlap.

### Fast Inference vs Strong Inference

Fast Inference works on an embedding that can fit into a variety of domains.
One such domain is the Strong Inference from LSTS.
So Fast Inference can work as a sophisticated driver for automated theorem proving in LSTS.

If Strong Inference is the bones, then Fast Inference is the muscle.

### LSTS example

Fast Inference works by coloring a frontier of potential inference rules to seek a balanced search space.
FI can be thought of as an intelligently weighted cost minimized search using Dijkstra's algorithm to expand the frontier.

First, the cost of inference rules is calculated.
Each inference is colored by it's internal path weight.
Path weight is calculated by the weight and color of each inference.
Sequenced inference is accounted for additively.

For example, a yellow pun with 3 options will get the color: 3yellow.
if it is followed in the body with an orange pun with 2 options, the cumulative weight would be: 3yellow 2orange.

Conditional or looped paths are accounted for additively.
So an if statement with 1red on the truthy path, and 2blue on the falsy path, would get a cumulative path weight of 1red 2blue.

Second, the frontier is cost minimized to search by minimum color.
A new path segment that does not expand the frontier max color is marginally free.
Max color is a single color weight of the maximum for each color across the entire frontier.
A new path segment that does expand the frontier max color has a cost equal to the new delta.
The frontier is expanded to minimize the maximum color value across all colors.
If one inference rule would expand to 3yellow and another would expand to 4orange, then the 3yellow is chosen first.

Third, once an intermediate objective has been reached, the cost to start from that node is reset to zero.

> If a lemma is true, then the lemma is free.



