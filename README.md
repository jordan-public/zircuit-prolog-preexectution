# Zircuit Pre-execution

## Abstract

Zircuit as Validium (popularly but imprecisely called ZK) Layer 2 EVM blockchain is transitioning from execution proof generation using Halo 2 to using Succinct Labs SP1.
Among other benefits, this presents an opportunity to offload most on-chain computation, move it off-chain and compress it to merely a ZK verification on-chain.
Instead of procedural we consider implementing this computation in a declarative language such as Prolog. This can produce an execution trace that would speculatively be understandable to AI models, such as the ones that can be used in Zircuit's pre-execution transaction Quarantine system. This arguably would 
improve the insightfulness in the process of detecting fraudulent transactions. On top of this, the pre-calculation can include assertions that could allow for pre-screened policies, in addition to the Zircuit's existing Customizable Transaction Policies for Institutions.

## Opportunity

Compare Aleo.

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

