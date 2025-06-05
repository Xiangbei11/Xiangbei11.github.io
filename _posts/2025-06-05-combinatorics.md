---
layout: post
title: "A Fairly Simple Math Problem from My Middle School Years — Still Useful Today"
date: 2025-06-05
description: A hands-on revisit of a classic math problem that uncovers surprising insights about counting, probability, and thinking before coding
math: true
#categories: [Mechanics, Physics]
#tags: [Stress, Strain, Mechanics, Material Science]

---


For reasons both universally acknowledged and personally unclear, I’ve always had a quiet fondness for math class in middle school. Recently, while working through a research problem, I ran into a version of a **Permutation and Combination** question that felt straight out of those school days. And it turned out to be surprisingly useful — not just for logic puzzles, but also for counting design combinations, modeling probabilities, and even generating constrained inputs in machine learning setups.

---

## The Problem (a simplified version, unrelated to the High Entropy Alloy work I'm actually doing)

You have **10 drawers arranged in sequence**, and each drawer can hold **one of five different colored balls** (say: A, B, C, D, E).

The constraint: **All five colors must appear at least once**.

- **Question 1**: How many valid arrangements are there?
- **Question 2**: Given that all five appear, if you pick one color at random, what’s the probability that it appears exactly 1, 2, … or 6 times?
- **Question 3**: Could these probabilities all be made equal by design? (Spoiler: No. We’ll see why.)

---

## Question 1 — Total Valid Combinations

If we ignore the constraint, each drawer can independently take on one of five colors:

$$
5^{10} = 9,\!765,\!625
$$

But we want only the combinations where **all five colors appear at least once**. This is where the **Inclusion–Exclusion Principle** comes in.

### Inclusion–Exclusion: What It Does

The Inclusion–Exclusion Principle helps calculate the size of a union of overlapping sets by correcting for overcounts.

Let’s say you have sets \(A_1, A_2, \dots, A_n\), and you want to count how many elements are in at least one of them:

$$
|A_1 \cup A_2 \cup \dots \cup A_n| = \sum |A_i| - \sum |A_i \cap A_j| + \sum |A_i \cap A_j \cap A_k| - \dots + (-1)^{n+1}|A_1 \cap \dots \cap A_n|
$$

In our case, we’re not looking for the union — we want to count how many **assignments include all 5 colors**, which is the complement of the union of the assignments **excluding at least one color**.

That gives us:

$$
T = \sum_{i=0}^{5} (-1)^i \binom{5}{i} (5 - i)^{10}
$$

Where:

- \(\binom{5}{i}\) = ways to choose *i* colors to exclude
- \((5 - i)^{10}\) = number of 10-drawer sequences using only the remaining colors
- \((-1)^i\) = alternates between subtracting and adding to adjust for overlapping exclusions

The binomial coefficient \(\binom{5}{i}\) is calculated as:

$$
\binom{5}{i} = \frac{5!}{i!(5 - i)!}
$$

Running this gives us:

**5,103,000** valid designs where all five colors are used at least once.

---

## Question 2 — Probability That a Given Color Appears *k* Times

Let’s say we fix one color, like “A.” What’s the probability that it appears **exactly k times**, for \(1 \leq k \leq 6\), in a valid design?

### Strategy

1. Choose \(k\) positions out of 10 to assign to “A”: \(\binom{10}{k}\)
2. Assign the remaining \(10 - k\) positions to the other 4 colors, ensuring each appears at least once

We use inclusion–exclusion again for this:

$$
P(K = k) = \frac{\binom{10}{k} \sum_{j=0}^{4} (-1)^j \binom{4}{j} (4 - j)^{10 - k}}{T}
$$

Where:

- \(T\) is the total valid designs from Question 1
- The numerator counts how many ways to place “A” exactly \(k\) times, and the rest validly among the other four

### Results

Each of these probabilities corresponds to a fraction of the form:

$$
P(K = k) = \frac{\text{Number of valid assignments where A appears } k \text{ times}}{T}
$$

Using that, we get:

- P(appears 1 time) = **0.365432** = 1,865,481 / 5,103,000
- P(appears 2 time) = **0.360000** = 1,837,080 / 5,103,000
- P(appears 3 times) = **0.197531** = 1,008,547 / 5,103,000
- P(appears 4 times) = **0.064198** = 327,607 / 5,103,000
- P(appears 5 times) = **0.011852** = 60,514 / 5,103,000
- P(appears 6 times) = **0.000988** = 5,771 / 5,103,000

---

## Question 3 — Can These Probabilities Be Made Equal?

Suppose we assume that all probabilities are equal and denote this common value by \(P\). That is,

$$
P(K = 1) = P(K = 2) = \dots = P(K = 6) = P
$$

Now let’s say we generate 1,000 valid samples. If each value of \(K = k\) appears \(d\) times, then:

$$
6 \cdot d = 1000 \quad \Rightarrow \quad d = \frac{1000}{6}
$$

This would also imply the total number of times color A appears across all samples is:

$$
1 \cdot d + 2 \cdot d + 3 \cdot d + 4 \cdot d + 5 \cdot d + 6 \cdot d = (1+2+3+4+5+6) \cdot d = 21d
$$

However, since there are 10,000 layer positions (1,000 samples × 10 layers) and the five colors must appear equally often, color A must occupy exactly:

$$
\frac{10,000}{5} = 2000 \text{ positions}
$$

So we would need:

$$
21d = 2000 \quad \Rightarrow \quad d = \frac{2000}{21}
$$

But earlier we said \(d = \frac{1000}{6}\). These two values of \(d\) are not equal:

$$
\frac{1000}{6} \ne \frac{2000}{21}
$$

This contradiction shows that our assumption — that all probabilities \(P(K = k)\) are equal — cannot hold under the constraint that each color appears equally across the dataset.

---

## Final Thoughts

At first, I didn’t give Question 3 much thought. I assumed it might be possible to make the probabilities equal, so I jumped straight into trying to write code for it. But halfway through, I realized it wasn’t working — not because of a bug, but because the logic didn’t hold. That’s when I stepped back and worked through the math.

Lesson learned: think it through before jumping into code — especially when the constraints are doing more than they seem on the surface.
