# baselion-jury-condorcet-voting

> **Condorcet / Schulze / Copeland voting methods applied to the aggregation of trading signals — with the academic references to back up when you would actually use each one.**

When you have *N* candidate strategies or signal variants and you need a single decision, averaging is the lazy answer. Social-choice theory has spent 200 years proving the lazy answer is bad. This library implements the serious answers — Condorcet winner, Schulze, Copeland, Ranked Pairs — and shows you how they behave on trading-signal ensembles.

This is an **educational** package. We use it internally in the jury mechanism of the [FOX OMEGA Engine](https://baselion.ai). The underlying weighting logic inside our production motor is not released, but the building blocks are.

## Install

```bash
pip install baselion-jury-condorcet-voting
```

## Worked example — aggregating 5 signal sources

```python
from baselion_jury import Ballot, schulze, condorcet_winner, copeland

# Each signal source ranks the 3 candidate actions (LONG, SHORT, FLAT)
ballots = [
    Ballot(["LONG", "FLAT", "SHORT"]),   # source A
    Ballot(["LONG", "FLAT", "SHORT"]),   # source B
    Ballot(["FLAT", "LONG", "SHORT"]),   # source C
    Ballot(["FLAT", "LONG", "SHORT"]),   # source D
    Ballot(["SHORT", "FLAT", "LONG"]),   # source E
]

print(condorcet_winner(ballots))  # may be None (Condorcet paradox)
print(schulze(ballots))           # always a winner
print(copeland(ballots))          # tie-tolerant
```

## Methods

| Method | Returns | Always-winner? | Notes |
|--------|---------|----------------|-------|
| Condorcet winner | single candidate or None | no | conceptually simplest, often None |
| Schulze | single candidate | yes | strongly Condorcet, monotonic |
| Copeland | single candidate or tie | mostly yes | simple pairwise-wins score |
| Ranked Pairs (Tideman) | single candidate | yes | Condorcet + independence of clones |

## Why this matters for trading signals

A naive majority vote across 5 models will happily ignore a clear Condorcet loser. In signal aggregation that can turn three weakly-positive votes into a FLAT recommendation when the pairwise truth is LONG. Schulze recovers the correct answer. The `/notebooks` folder walks through a synthetic example where this changes your Sharpe ratio meaningfully.

## References

* Schulze, M. (2011). *A new monotonic, clone-independent, reversal symmetric, and condorcet-consistent single-winner election method.* Social Choice and Welfare.
* Tideman, T. N. (1987). *Independence of clones as a criterion for voting rules.* Social Choice and Welfare.
* Condorcet, M. de (1785). *Essai sur l'application de l'analyse à la probabilité des décisions rendues à la pluralité des voix.*
* List, C., & Goodin, R. E. (2001). *Epistemic democracy: Generalizing the Condorcet Jury Theorem.* Journal of Political Philosophy.

## Status

Pedagogical / research quality. Sufficient for signal aggregation, not for governance-critical voting systems.

## License

MIT.

---

## About Baselion

Baselion is an institutional-grade quantitative trading intelligence platform, powered by the proprietary FOX OMEGA Engine. Learn more at [baselion.ai](https://baselion.ai).
