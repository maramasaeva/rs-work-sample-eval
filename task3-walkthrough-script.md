# TASK 3 — what I say
~2 min. Open: results/scores.json, score_v2.py.

---

I ran the scorer first. Both models got 0.933. The same number, twice. Two
errors out of thirty. So an eval built to compare two systems couldn't tell
them apart.

[scores.json]

The main problem is how one answer gets scored. Each question has one blob of
reference text and the judge gives one verdict for all of it. The German
registration item is about four separate facts. The Finnish one is a single
date. Both are worth one bit.

That's what produced the result. Both models got the registration item mostly
right and left out a deadline. There's nowhere to put "mostly right", so the
judge called it incorrect and suppressive, while its own reasoning said it was
a missing detail. That one item is basically the whole result.

[score_v2.py]

I wrote a second scorer next to the original so you can diff them. I stopped
showing the judge the failure trap, because it was being told what the wrong
answer looks like before it read the answer. I made it grade fact by fact, so
a missing deadline shows up as "three of four" instead of a coin flip. And I
made the source check deterministic, because that's the only number separating
the two models and I didn't want it from the same judge I'm suspicious of.

It doesn't produce a number yet. It won't run until the claim lists are
written, and I didn't want the agent writing those, because that's the actual
definition of what counts as correct.

So the tradeoff: I worked on what gets measured instead of getting a new number
out. A cleaner number over the same broken rule is just a more confident wrong
answer.

The agent got two things wrong, both caught by reading. It put a three percent
error rate in my task one write-up with no source. I cut it. And its first
ranking was all reporting fixes: the denominator, the controls, intervals. All
fine, all surface. The real problem was one step earlier. It tidied the number
before asking if the number was the right shape.

What I'd do differently: run the scorer in the first two minutes. Seeing 0.933
twice is what reframed everything and I got there slowly.

With more time, the real cap is that the judge isn't validated. Every finding I
just gave you came from the instrument I'm suspicious of. I'd label all thirty
myself, blind, and report plain agreement next to kappa.

And I'd want people on this. I learnt a lot of this today. Someone who knows
election law should check my ground truth, and a second person should label the
same thirty items. Right now I'm the only labeller and I also wrote the rubric,
which isn't a position I should be in.

Next thirty minutes: write the claim lists, run v2, get a number to put next to
0.933.

---
- 0.933 isn't a finding. it's one unvalidated judge on 15 questions.
- v2 hasn't been run. don't say it improved anything.
