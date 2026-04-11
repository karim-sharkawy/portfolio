---
layout: post
title: "In Praise of Simple Solutions"
date: 2024-03-15
category: "Creative"
---

Engineers love complex solutions.

We build pyramids when the problem needs a cart.

---

I spent a week on a recommendation algorithm.
Learned everything: collaborative filtering, matrix factorization, deep learning.
Implemented it. Shipped it. Performance: +3% engagement.

Then someone asked: "What if we just showed them popular items?"

Simple algorithm. Built in an hour.
Performance: +2.8% engagement.

We shipped the simple one.

---

The complex solution *was* better.
By 0.2%.

The costs:
- 7 days of development
- Complex codebase (harder to maintain)
- More infrastructure
- Harder to debug when it breaks
- Impossible to explain to non-ML people

The simple solution traded 0.2% accuracy for simplicity, speed, debuggability.

That was the right tradeoff.

---

I think there's a hidden bias in engineering.

Complexity looks like progress.
Simplicity looks like we didn't try hard enough.

But the opposite is often true.

The best engineers I know:
- Solve problems with 3 lines when others use 3000
- Explain decisions to 5-year-olds
- Ship in days what others need months for
- Delete code more than they write

---

There's a quote attributed (falsely, probably) to Einstein:
*"Everything should be as simple as possible, but not simpler."*

That's not quite right though.

The rule should be:
*"Everything should be as simple as the problem requires, and no simpler."*

---

Occam's Razor for code:
Do not multiply solutions beyond necessity.

If a for-loop solves the problem, don't use a library.
If a simple average works, don't over-engineer.
If 90% accuracy is enough, don't chase 92%.

Ship simple. Measure. Optimize only what matters.

---

The irony:

Simple solutions require *more* thought.
Less code, but more understanding.
Constraints breed clarity.

Complex solutions are easier to build.
Just keep adding features until it works.
But they're harder to live with.

---

Dear engineer:

The most impressive thing you can do isn't building something complex.

It's building something *simple that works*.

---

*"The most powerful tool we have as programmers is the ability to not write code."*
— Jamie Brandon
