## MAP IV: Mathematics as a Language - I

# Is Math Invented or Discovered?

Whether mathematics is discovered or invented is an age-old question but it could also have been an innocent false dilemma all along. Could it be something in between or beyond?

An alternative approach to answering this could be that mathematics is a formal language that is constructed using formal structures and operations on those structures. More precisely, mathematics is a system for deciding whether mathematical statements about a mathematical structure are true. 

So what could make a statement mathematical? Tautologically, any statement that follows mathematical rules could be called a mathematical statement. 

Is that last sentence in itself an exhaustive rule? That is, are there other ways to write or construct a mathematical statement? Mathematics can sometimes be about something more than a system of rules or instructions. In fact, this simple rule-based 'kind' of mathematical statements is usually a small subset of mathematical statements that map directly to the kind of statements we write in imperative programming. As an example of this think of the following multiplication as a 'mathematical statement':

```
 1234
 x 56
______
 7404
61700
______
69104
```

We teach this kind of lifeless and tedious instruction set to our kids to teach them multiplication when they are starting to learn mathematics. And of course, most of them grow up to hate some parts or all of mathematics. Well there are some that do not mind these punishments. And that's how you have imperative programmers or drone-workers. However, these instruction sets in arithmetic come from the algebra of numbers, which is slightly more abstract than merely adding 1234 to itself 56 times as an arithmetician. Such algebras form mathematical structures as opposed to mere mathematical rules. 

## One of the Rings

![ring.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603974856730/Ae7HOzkKM.png)

Integers form a useful structure called a ring. And a ring is a set that just so happens to have two operations,  `+` and `·` , defined on its elements and the second one, which is `·`, happens to be distributive. Furthermore, it has both right and left distributivity. With this information, we know that:

```
1234 · 56 

= 1234 · (50 + 6) 
= (50 + 6) · 1234 

= 1234 · 50 + 1234 · 6
= 1234 · (5 · 10) + 1234 · 6

= (1 · 1000 + 2 · 100 + 3 · 10 + 4 · 1) · 5 · 10 
+ (1 · 1000 + 2 · 100 + 3 · 10 + 4 · 1) · 6

= (1 · 5 · 1000 + 2 · 5 · 100 + 3 · 5 · 10 + 4 · 5 · 1) · 10 
+ (1 · 6 · 1000 + 2 · 6 · 100 + 3 · 6 · 10 + 4 · 6 · 1)   

= 61700 + 7404
= 69104
```

In the first two pairs of steps, we demonstrate that integers have left and right distributivity under the second operation `·`. In the subsequent two lines, we use associativity of this same operation i.e. 

```
1234 · (5 · 10) 
= 1234 · 5 · 10
= (1234) · 5 · 10
``` 

Then we substitute 1234 with `1 · 1000 + 2 · 100 + 3 · 10 + 4 · 1`. Can we take it for granted that such a substitution is allowed? If we are being serious about the grammar of our language, we cannot just assume that such a substitution is allowed. 

## Digression into Programming

We have mentioned, albeit not clarified, in previous posts in the MAP series, that propositions and types are isomorphic. So in a sneaky typed-programmer way, we can say that this substitution is allowed as long as `+` is a pure operation and, therefore, referentially transparent. Purity simply means that for the same two operands, the operator always returns the same value and nothing else - not even any hidden consequence or side-effect. Referential transparency for an expression like `30 + 4` means that the expression is replaceable with the value of the expression e.g. `34`. How are we sure that merely 'purity' preserves this 'replaceability'? 

Well, it is because we are guaranteed that along with the purity, we also have the rule that rings are closed under both operations i.e. `+` and `·` are always going to produce an integer result, in the case of the integer ring, or a real result, in case of the more general polynomial ring. So we know that the types of both operands and the results are the same. This is already good enough but insufficient support in our claim for allowing substitution. The more important question however is how are the semantics of `+` or `·`operation defined?

## Let's Play the Peano

For this, we have to build numbers themselves. If we are building up numbers from scratch it is easy to construct them from the Peano axioms (rule-based) or from Church numerals (function-based). Once you have integers and define how various operations are defined on integers, it is quite easy but not trivial to construct the reals. 

A Peano number is either a `0` or a successor of another Peano number. In Haskell, we define Peano number values like this:

````
data Peano = Zero | Succ Peano
````

Obviously, `Zero` could correspond to the natural number `0`. Do we have 1 in this system yet? It could not be the first rule, which only produces `Zero` and it can only be one of these two rules but not both. This is because the pipe `|` represents a disjoint union or an 'either-or' mode of selection in this case. As for the second rule, `Zero` is the only Peano number we know so far. So the second constructor `Succ` has only `Zero` as a candidate thus far. Note that `Zero` has type `Peano`, and `Succ` has type `Peano -> Peano`  i.e. it takes a Peano number and returns a Peano number. We can alternatively say that `Succ` is closed under Peano numbers. In that sense, Peano numbers are instead of 0, 1, 2, 3, 4... simply `Zero`, `Succ Zero`, `Succ (Succ Zero)`, `Succ (Succ (Succ Zero))` and so on and so on. 

Note that we have not defined any rule that allows for any Peano number `x` such that `Succ x` evaluates to `Zero`. This is because we do not have negative numbers (yet) and thus `Zero` is not the successor of any Peano number. We can still define ring-like operations on these numbers but without the inverse operations, we only have a semi-ring or just 'rig' - a ring without the 'negative' operations. 

## Take me to Church

Peano numbers define numbers in terms of either a Zero or repeated application of `Succ` initially on `Zero` and then on any subsequently formed Peano numbers. In the same way, Church numerals are defined in terms of how many applications of a function is needed to 'reach' or count to that number. This is shown in this handy Wikimedia image below:

![churchnumbers.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603979240487/A2_9Gnc-b.png)

We will examine the lambda expressions on the right in further detail in the later posts in this series. The important point to note is that a number is essentially the count of how many times a function had to be applied to a base number, which is again 0 in this case too. Note that we could have started the count at 1 instead of 0 like a heathen. It is more useful to not do something reproachable of that sort.

Do we have a formal semantics of `+` and `·` using this foundation for numbers? Turns out we are quite close. Even the unary `Succ` above can be thought of as a `+` operation with one of the operands `1`. That is, `Succ Zero` is the same as `0 + 1` in our above school-level arithmetic that we started with. However, please note that it is not necessary to stick to a `Succ` as boring as that. We can also just as well define `Succ a` to be equivalent of `a + x` where `x` is an arbitrarily chosen but fixed Peano number. In fact, `Succ a` could just as well be `a - x` (which would generate negative numbers) or even `2^a` (which would generate powers of base 2). Both formulations of `Succ` would lead to a set of 'numbers' of the same size - even though that size or cardinality is the same countable infinity. However, we have not formally defined such addition, subtraction, or exponentiation yet so using any of the examples in this paragraph to define `Succ` would be cheating.

In this case, it is actually easier to not cheat and simply write the definition. Let's start with the notion of what it means to have a number `n` in our system:

![fnlambda.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603988713398/_GTAM1k0x.png)

As seen in the last two images, 0 is simply the result of no applications of f. 1 is the result of one application of `f`. 2 is the result of `f f`. 3 is the result of `f f f` and so on and so on. We use this to motivate the intuition behind the notion of `+`:

![fmplusn.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603989267577/9V9EtoEow.png)

If you have `n` applications of `f` lying around and I bring you `m` applications of `f`, and if we compose those together, we have `m + n` number of applications of `f`. According to our rule, `m + n` many applications of `f` results in the number `m + n`. Note that `+` is closed under addition. So we know that the result of the expression `m + n` is the same as the value `m + n`, which is still a Church numeral. So can we now define `*`? It is actually even easier.

![fmtimesn.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1603989728245/Wxxtd31UX.png)

If you have `n` applications of `f` lying around, just apply all of that `m` many times. That result is `m * n`. 

## Invented or Discovered?

So have we invented addition and multiplication for integer/polynomial rings or Church/Peano numbers? We probably cannot claim to invent that which has pre-existed our invention of it. So did we discover them instead? This is a slightly more acceptable proposition because we did 'run into' these operations. In the same sense that Columbus 'discovered' the Americas by getting there after so many other explorers and settlers, we did 'discover' addition and multiplication from our first principles. 

A better approach to answering this question would be that we have neither invented nor discovered any mathematics. We have simply constructed a formal* language with which addition and multiplication are defined both intensionally and extensionally. We started with setting out basic rules like what it means to have a number and then we decided what we can do with the set of that numbers. We constrained ourselves to map from a number to a number instead of mapping a number to a fruit or a planet. We thus required a closed nature of operations. We then added further constraints about what the unary `Succ` or `f` can do. This unary operation allowed us to define what it means to have something after the base number `Zero` or `0`. 

`f (f Zero)` is 2 applications of `f`. Similarly, `f (f (f Zero)))` is 3 applications of `f`. We then thought of what it would mean to compose the first number with the second one. Such a composition gives us `f (f (f (f (f Zero))))` or 5, which we defined as an addition. If we had just repeated the first number as many times as the value of the second number, we would have  `f (f (f (f (f (f Zero)))))` or 6. The reason addition and multiplication behave this way is that we defined them to be exactly like this. Thus we have just been creating structures (e.g. the ring **Z**) and then defining what operations are allowed (e.g. **+**, **·** ). We can do something similar when playing a logical game like the zero-player game called **[Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)** with the following production and termination rules for cells in an infinite grid of rectangular cells.

1. Any live cell with two or three live neighbours survives.
2. Any dead cell with three live neighbours becomes a live cell.
3. All other live cells die in the next generation. Similarly, all other dead cells stay dead.

Interestingly this allows for us to create a complete Turing machine on merely a cellular grid. This means that we have a computer with just 3 foundational rules (or first principles) and that anything that can be computed at all can be computed with just this eccentric grid (structure) and the introduction/elimination rules (operations) just like we did with the ring and its two essential and invertible operations. Is it better to say that the late John H Conway discovered the Game of Life or that he constructed it? 

I bet it is the same answer for whether this great mathematician, or any other for that matter, discovered or constructed mathematics.