---
title: Some notes on learning Lean
date: 2024-05-07
tags:
- lean
---

This post contains my notes of learning Lean, following the [Functional Programming in Lean](https://lean-lang.org/functional_programming_in_lean/title.html) book by David Thrane Christiansen.

### Function

```lean
def joinStringsWith (delim : String) (s1 : String) (s2 : String) : String :=
  String.append s1 (String.append delim s2)

#eval joinStringsWith ", " "one" "and another"  --> "one, and another"

#check fun (x : Nat) => x + 5 -- Nat → Nat
#check λ (x : Nat) => x + 5   -- Nat → Nat
```

### Structure

```lean
structure Point where
  x : Float
  y : Float
deriving Repr

def origin : Point := {x := 0, y := 0}

structure Point2D where
  point :: -- constructor
  x : Float
  y : Float
deriving Repr

#eval Point2D.point 1.0 2.0
```

### Inductive Type

```lean
inductive MyNat where
  | zero : MyNat
  | succ (n : MyNat) : MyNat
```

### Pattern Matching

```lean
def even (n : Nat) : Bool :=
  match n with
  | Nat.zero => true
  | N.succ k => not (even k)

def even : Nat -> Bool
  | 0 => true
  | n + 1 => not (even n)
```

### Polymorphism

```lean
structure PPoint (α : Type) where
  x : α
  y : α
deriving Repr

/- Implicit arguments -/
def listHead? {α : Type} (xs : List α) : Option α :=
  match xs with
  | [] => none
  | y :: _ => some y
```