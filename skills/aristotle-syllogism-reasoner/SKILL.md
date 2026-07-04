---
name: aristotle-syllogism-reasoner
description: Guides interactive reasoning through Aristotelian categorical syllogisms—collects premises, identifies terms, converts to categorical form, and tests deductive validity with Venn-style counterexamples. Use when the user asks about syllogisms, categorical logic, major/minor premises, middle terms, deductive validity, or whether a conclusion follows from premises.
---

# Aristotle Syllogism Reasoner

Interactive, educational reasoning for Aristotelian **categorical syllogisms**. Focus on **logical validity**, not whether premises are factually true in the real world—unless the user explicitly asks for factual evaluation.

## Tone

Simple, precise, educational, and interactive. Ask one clear question at a time when collecting input. Explain reasoning in plain language.

## Categorical forms

Only these four proposition types:

| Form | Pattern |
|------|---------|
| Universal affirmative (A) | All A are B. |
| Universal negative (E) | No A are B. |
| Particular affirmative (I) | Some A are B. |
| Particular negative (O) | Some A are not B. |

When converting user text, normalize to one of these. If wording is ambiguous, ask a clarifying question before proceeding.

## Workflow

Follow these steps in order. Do not skip ahead until premises are clear.

### Step 1 — Major premise

Ask the user for the **major premise**: the **general rule** (typically about the broader category or predicate class).

Example prompt: *What is your major premise—the general rule?*

### Step 2 — Minor premise

Ask the user for the **minor premise**: the **specific case** (typically about the subject of the conclusion).

Example prompt: *What is your minor premise—the specific case?*

### Step 3 — Conclusion source

Ask whether the user wants to:

- **Provide a conclusion to test**, or
- **Have the skill infer the conclusion automatically**

If inferring, derive the strongest categorical conclusion that the premises support (or state that no valid particular/universal conclusion follows).

### Step 4 — Convert to categorical form

Rewrite each premise and the conclusion (given or inferred) into standard categorical form using the four patterns above. Show the converted forms internally before analysis; include them in the final output.

### Step 5 — Identify terms

| Term | Role |
|------|------|
| **Minor term (S)** | Subject of the conclusion |
| **Major term (P)** | Predicate of the conclusion |
| **Middle term (M)** | Term appearing in both premises but **not** in the conclusion |

Arrange premises in **standard order** for analysis:

1. **Major premise** — contains P and M (predicate rule)
2. **Minor premise** — contains S and M (specific case)

If the user's original order differs, reorder silently and note the standard form in the explanation.

### Step 6 — Test deductive validity

A syllogism is **valid** iff it is **impossible** for both premises to be true while the conclusion is false.

A syllogism is **invalid** iff there exists **at least one possible case** where both premises are true and the conclusion is false.

### Step 7 — Venn-diagram-style reasoning

Use set-theoretic (Venn) reasoning:

1. Draw or mentally model three overlapping circles: **S**, **M**, **P**.
2. Shade or mark regions excluded by each premise (e.g., "No M are P" shades the M∩P overlap).
3. Mark existence for particular premises ("Some S are M" requires at least one element in S∩M).
4. Check whether the conclusion **must** hold in every diagram consistent with the premises.
5. If **any** consistent diagram makes the conclusion false, the argument is **invalid**.

For invalid arguments, produce a **concrete counterexample**: assign a small domain (people, animals, shapes) where premises are true and the conclusion is false.

## Recognized valid patterns

These moods are always valid (M = middle, S = minor, P = major):

```
All M are P.   All S are M.   ∴ All S are P.        (Barbara)
No M are P.    All S are M.   ∴ No S are P.         (Celarent)
Some S are M.  All M are P.   ∴ Some S are P.       (Darii)
Some S are M.  No M are P.    ∴ Some S are not P.   (Ferio)
All S are M.   No M are P.    ∴ No S are P.         (Cesare / related)
```

Also valid when equivalent reorderings preserve standard figure and mood (e.g., EAE-1, AII-1, EIO-1, AEE-2).

## Recognized invalid patterns

These are **always invalid**—do not treat them as valid even if the conclusion happens to be true:

```
All A are B.  Some C are B.  ∴ Some C are A.
All A are B.  All C are B.  ∴ All C are A.
Some A are B. Some B are C. ∴ Some A are C.
```

Explain invalidity by:

- **Missing connection** — the middle term is not distributed properly, or premises do not link S to P through M.
- **Counterexample** — a concrete scenario where premises hold and the conclusion fails.

Common failure modes to mention when relevant:

- **Undistributed middle** — M not linked across both premises
- **Illicit major/minor** — term distributed in conclusion but not in its premise
- **Two negatives** — cannot derive a valid affirmative conclusion
- **Two particulars** — cannot derive a valid universal conclusion
- **Affirmative premises, negative conclusion** — requires one negative premise

## Output format

Return results in **exactly** this structure:

```
Major Premise:
Minor Premise:
Conclusion:
Validity: Valid / Invalid
Explanation:
Corrected Conclusion: [only if useful—omit this line when not needed]
```

**Explanation** must:

- State the identified S, M, and P
- Give the categorical mood (e.g., AAA-1, EAE-1)
- For **valid** arguments: briefly show why no counterexample exists
- For **invalid** arguments: show the missing link or a counterexample
- Avoid judging real-world factual truth unless the user requested it

**Corrected Conclusion** — include only when:

- The user's supplied conclusion is invalid but a different valid conclusion follows, or
- A stronger/weaker categorical conclusion is the correct inference

## Interactive behavior

- If premises are incomplete, contradictory, or not categorical, ask targeted follow-up questions before analyzing.
- If the user provides all three statements at once, still confirm major vs. minor premise if unclear.
- Offer to walk through the Venn reasoning step-by-step when the user asks to learn, not only when reporting validity.
- When the user asks *"is this true?"* about the real world, distinguish **validity** (logical form) from **soundness** (premises actually true)—answer only what they asked.

## Quick reference — term placement

Standard figure-1 layout:

```
Major:    M — P   (e.g., All M are P)
Minor:    S — M   (e.g., All S are M)
Conclusion: S — P (e.g., All S are P)
```

The middle term **M** appears in both premises and never in the conclusion.
