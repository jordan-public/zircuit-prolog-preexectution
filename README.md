# Zircuit Pre-execution

## Abstract

Zircuit as Validium (popularly but imprecisely called ZK) Layer 2 EVM blockchain is transitioning from execution proof generation using Halo 2 to using Succinct Labs SP1.
Among other benefits, this presents an opportunity to offload most on-chain computation, move it off-chain and compress it to merely a ZK verification on-chain.
Instead of procedural we consider implementing this computation in a declarative language such as Prolog. This can produce an execution trace that would speculatively be understandable to AI models, such as the ones that can be used in Zircuit's pre-execution transaction Quarantine system. This arguably would 
improve the insightfulness in the process of detecting fraudulent transactions. On top of this, the pre-calculation can include assertions that could allow for pre-screened policies, in addition to the Zircuit's existing Customizable Transaction Policies for Institutions.

## Prior Work

For full disclosure, I attempted to implement a simplified Prolog interpreter in Cairo for deployment on the Starknet blockchain during a previous ETHGlobal hackathon. However, due to Starknet’s computational-budget constraints at the time and Cairo’s lack of efficient data structures, I abandoned the project and haven’t revisited it since.

## Introduction

### Pre-execution: the example of Aleo

Let us look at [Aleo](https://developer.aleo.org/concepts/network/core_architecture), as an example of off-chain pre-computing. For the sake of this explanation
let us ignore the privacy aspect and assume that all computation is public.
This compression of computation has been known for decades (reference needed), but let us look at it from the blockchain aspect.

![aleoarch]()

The programs in Aleo's language Leo have two parts for each entry point:

- "transition" - this part is executed off-chain. All necessary data on which it operates is
passed to it as parameters. If any part of that comes from the blockchain state, it is
pre-fetched before it is passed as parameter(s). The transition creates a proof of computation, and then it passes it to the next stage along with the public inputs, to be completed on-chain.
- "async function" - this part is executed on-chain. It receives the proof and the public inputs from the "transition". After the verification of the proof, it uses the public inputs as parameters to the on-chain function execution. This usually includes matching of
the on-chain state against the pre-fetched data and recording of the state changes.

In the above, most of the computation work should be completed off-chain in the "transition", relieving the blockchain of the heavy work.

If we look at Zircuit, all on-chain work proven and the new state and proofs are recorded
anyway, so off-loading the work off-chain would not add overhead in any case. We argue that it would actually reduce load, especially if the off-chain work is recorded as
execution trace, with all non-deterministic search (or guesswork) is eliminated from it.

### Prolog and Non-determinism

Prolog is a very simple declarative programming language. It's name stands for "Programming in Logic". Prolog programs are collections of "sentences" that describe
definitions of "predicates". For example, the predicates "parent" and "grandparent"
can be defined as:

```
grandparent(A, C) :- parent(A, B), parent(B, C).

parent(adam, seth).
parent(seth, enosh).
parent(adam, cain).
```

The upper case letters are "variables" which can be instantiated with terms such as numeric or literal constants (```5``` or ```adam```), or complex terms such as ```t(5, adam)```.

The above program means:
$(\forall A, C, B: grandparent(A, C) \vee \lnot parent(A, B) \vee \lnot parent(B, C)) \wedge$<br/>
$parent(adam, seth) \wedge parent(seth, enosh) \wedge parent(adam, cain)$

The execution of the program starts with a query such as:
```
?- grandparent(X, enosh).
```
which responds with:
```
X = adam
```
and we can ask for alternative solutions if there is more than one, but that is beyond the point of our discussion.

Prolog as such is incomplete, as it's clauses are of the form:
```
A :- B, C, ...
```
cannot express everything first order predicate logic can. Kowalski (Kowalski, Robert. “Logic for Problem Solving.” Elsevier Science Ltd, 1979) has shown that any first order logic
statement can be expressed as a set of so called "Horn Clauses" of the form:
```
A, B, ... :- C, D, ... 
```
Each such Horn Clause can be translated into the usual logic symbolic:

$A \vee B \vee ... \Leftarrow C \wedge D \wedge ... $

and each of these ```A, B, C, D``` is in the form of the Prolog predicates. To remedy
this, Prolog introduces the ```!``` ("cut") operator which aborts the search for solution.
Prolog performs its search for solution in a depth-first manner. This makes Prolog
as expressible as First Order Predicate Logic under the assumption of "what we cannot
prove is not true" inside its programs.

So, for example "not" is defined as (not exactly correct Prolog syntax):
```
not(X) :- X, !, fail.
```

The execution of Prolog consists of the following:
- **Unification** of terms such as ```x(a, X, Y)``` and ```x(Z, b, Y)``` resulting in ```x(a, b, Y)```.
- **Searching** for match of the current predicate we are trying to match against clauses that define that predicate in the program.
- **Abortion** of search in the current branch once the ```!``` operator is encountered.

This represents a Turing-complete execution. Yet, this search can involve a lot of computation, as the Prolog programs include non-determinism as in the definition of ```parent(X, Y)``` above.

It is important that the result of the Prolog execution can be expressed as **Execution Trace** which can be much smaller than the number of steps in the search, as it only
contains the successful branch of the search. Note that in the Execution Trace the
unsuccessful branches that lead to a failure after ```!``` have to be included as well. Yet, this does not impose excessive overhead. This part of the discussion can be expanded,
but it is well known.

## Opportunity



## Problem

Zircuit - quarantine

AI - unreliable / unpredictable
I would be worried to lend in protocol where AI can block liquidations

Customizable Transaction Policies for Institutions - defeats purpose of crypto
    fictitious court process
    institutions misbehave
    congress members misbehave
Permissionless version of this would be OK

How about pre-deposits

- If human decides - back to square 1
- If proof decides - OK ...

## Opportunity

ZK Pre-computation
Prolog -> Horn Clauses -> Trace Proof -> On-EVM verification (needed, or just aggregation?)

## Proposed Solution

Pre-execute: all inputs are constants, all read-only.
ZK proof of pre-execute

