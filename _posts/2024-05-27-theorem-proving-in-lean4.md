---
title: Theorem Proving in Lean 4
date: 2024-05-27
tags:
- lean
---

This post contains my notes of the [Theorem Proving in Lean 4](https://leanprover.github.io/theorem_proving_in_lean4) book _by Jeremy Avigad, Leonardo de Moura, Soonho Kong and Sebastian Ullrich, with contributions from the Lean Community_.

## Chapter 3: Propositions and Proofs

Implication.

```lean
example : p → q :=
  fun h : p => sorry -- proof of q given h
```

Conjunction, a.k.a. and.

```lean
fun h : p ∧ q =>
  have hp := h.left
  have hq := h.right
  sorry  -- rest of proof using hp and hq

⟨hp, hq⟩ : p ∧ q  -- as proof of p ∧ q
```

Disjunction, a.k.a. or.

```lean
fun h : p ∨ q =>
  h.elim
    (fun hp => sorry)  -- proof using hp
    (fun hq => sorry)  -- proof using hq

Or.inl hp : p ∨ q   -- prove (p ∨ q) using hp
Or.inr hr : p ∨ q   -- prove (p ∨ q) using hq
```

Equivalence, a.k.a. iff.

```lean
example : p ↔ q :=
  Iff.intro
    (fun hp : p => sorry) -- proof of q given p
    (fun hq : q => sorry) -- proof of p given q

fun h : p ↔ q =>
  have hl : p → q := h.mp
  have hr : q → p := h.mpr
```

Negation `¬p` is defined as `p → False`. We prove `¬p` by deriving a contradiction from `p`.

```lean
-- Prove ¬p with definition.
example : ¬p :=
  fun hp : p => show False from sorry

-- Falsity can be derived from a contradiction
example (hp : p) (hnp : ¬p) : False := False.elim (hnp hp)

-- Falsity can imply anything
example (hf : False) : p := False.elim hf
example (hp : p) (hnp : ¬p) : q := absurd hp hnp
```

Auxiliary subgoals

```lean
suffices hp : p from sorry -- Proof of original proposition assuming p is true
show p from sorry          -- Proof of p
```

Classical logic: excluded middle

```lean
open Classical

Or.elim (em p)
  (fun hp : p => sorry)
  (fun hnp : ¬p => sorry)

example (h : ¬p) : q :=
  byContradiction
    (fun h1 : ¬q =>
      have h2 : p := ... -- given ¬q, prove p
      show False from h h2)
```

## Chapter 4: Quantifiers and Equality

The universal quantifier `∀`

```lean
-- introduction --
example : ∀ x : α, p x :=
  fun x : α => -- prove (p x) here

-- elimination --
fun h : ∀ x : α, p x => -- Given (y : α), we can now use (h y) as a proof of (p y)
```

The existential quantifier `∃`

```lean
-- introduction --
example : ∃ x : α, p x := Exists.intro w hw  -- w is a particular value in α, hw is a proof of (p w)
example : ∃ x, p x := ⟨w, hw⟩

-- elimination --
example (h : ∃ x : α, p x) : q :=
  match h with
  | ⟨w, (hw : p w)⟩ => q  -- w is a particular value in α, hw is of type (p w)

example : (∃ x : α, p x) → q :=
  fun ⟨w, (hw : p w)⟩ => q
```
