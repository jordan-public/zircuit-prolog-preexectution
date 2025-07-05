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

which responds 

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

