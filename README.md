# Three ways a proof fails

Three repositories, three domains, one argument. Each one measures something,
and each one is really about **the measurement rather than the thing measured.**

The claim is not that these systems were wrong. It is that **the proof was
wrong, in a way that produced a confident number.** A broken system is loud. A
broken proof is quiet, and it agrees with you.

| | Domain | The measured failure |
|---|---|---|
| [aml-agent-evaluation](https://github.com/IvanSFlowGit/aml-agent-evaluation) | evaluating an AI decision agent | precision reported at **8.3%** when the real figure was **91.7%** |
| [prompt-decomposition](https://github.com/IvanSFlowGit/prompt-decomposition) | auditing an accreted system prompt | **28 rules, 7 overriding an earlier rule, 6 of them silently overriding an earlier one**, and one input returning opposite decisions on two runs |
| [error-policy-matrix](https://github.com/IvanSFlowGit/error-policy-matrix) | testing a framework's error handling | **6 of 24** on one build, **24 of 24** on another, with 6 of 6 controls still raising in both |

---

## 1. The yardstick was built out of the belief it was meant to test

**aml-agent-evaluation.**

A truth set was assembled to measure how well an alert-triage agent decided.
It was assembled by the person who already had a view about which alerts
mattered. Everything outside the known case was assumed to be a false positive.

Precision came back at **8.3 percent.** Reading the items rather than the score
put it at **91.7 percent.**

Nothing malfunctioned. The arithmetic was right, the pipeline ran, the number
was reproducible. **The set was the defect**, and a set built from a belief will
report that belief back at whatever precision you like.

**The failure mode: a measurement whose ground truth encodes the hypothesis
cannot disconfirm it.** The tell is that the labelling and the conclusion came
from the same head, and nobody read the items.

## 2. Reading the rules is not running them, and one run is not running them

**prompt-decomposition.**

A system prompt was grown the way real ones grow: one incident at a time,
appended, never edited, never read back for conflicts. Then it was decomposed
into a rule table and the rules were compared against each other.

**28 rules. 7 of them change the meaning of an earlier rule and 6 do it without
naming the rule they act on. 130 pairs compared. Two contradictions.**

The rules that disagree are never adjacent, are never individually wrong, and
only disagree on the case neither of them names. Reading top to bottom does not
surface them, because each rule is correct on its own page.

Then the prompt was **run** against the conflicting inputs. On one contradiction
the same input returned **SEND** and **DO NOT SEND** on two consecutive runs of
the same model on the same day. On the other, **two different models resolved it
in opposite directions**: the small one honoured a discount the store never
issued, the frontier one refused a remedy the prompt promised in writing.

**The failure mode: the artefact and its behaviour are different objects.** A
table finds contradictions. Only running finds disobedience. And a single run
reports one of several possible answers as the answer, which is why the
consistent model is not safer than the flickering one, only quieter.

## 3. Zero is not a result until it can name its own cause

**error-policy-matrix.**

A fixture measuring whether a framework's error-handling policies behave
consistently across execution shapes. Two things in its design carry the whole
argument, and neither is about LangGraph.

**Controls run on every invocation and are reported separately.** 24 handled
cases that must not escape, and **6 over-suppression controls that must still
raise.** Without those six, a run where everything is suppressed looks identical
to a run where everything is handled. The green would mean nothing and would
look exactly like the green that means something.

**A fixture that cannot execute is a third state, not a failure.** Exit 0 pass,
exit 1 fail, **exit 2 could not run.** Verbatim from the repository: *"A fixture
that cannot execute produces zero passes, and zero passes reads exactly like a
broken framework when the truth is a broken fixture."*

The numbers, each tied to a named build rather than to the framework in general:
**6 of 24 handled cases pass on 1.2.11 (644815f), 24 of 24 on PR #8611
(adfa8d0f), 6 of 6 controls raising in both.**

**The failure mode: a null result that cannot distinguish the instrument's
failure from the finding.** Zero is simultaneously the most likely output of a
broken measurement and its most publishable one.

---

## The position

**A proof is an instrument, and instruments fail in ways that look like
results.**

Three mechanisms, three domains, measured rather than argued:

1. **The yardstick made from the hypothesis.** It cannot disconfirm.
2. **The reading substituted for the running.** The document is not the system.
3. **The null that cannot name its own cause.** Zero is ambiguous until the
   instrument proves it ran.

Each one produces a number that is defensible, reproducible, and wrong. None of
the three is detectable by looking harder at the output, because the output is
exactly what a correct run looks like. All three are detectable in one move:
**carry a control that could have disagreed with you, in the same run, and
report it separately.**

## What this is not

It is not a claim that these frameworks or models are bad. Every number here is
tied to a build, a date and a method, and the repositories carry the run
conditions so a stranger can reproduce or refute them.

It is also not three case studies looking for a theme. The AML set was measured
in July, the framework matrix in August, the prompt decomposition after it, and
the same defect turned up in all three without being looked for. **That is the
only reason it is worth writing down.**
