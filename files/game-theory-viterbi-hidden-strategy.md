# Game Theory & Viterbi: The Hidden Strategy Revealed

Great framing — because the connection is deep and non-obvious. Let me rebuild the entire thing from a game theory perspective.

---

## 🎭 The Core Reframe

In the HMM story, **nature is your opponent**.

| HMM Concept | Game Theory Equivalent |
|---|---|
| Hidden states | Your opponent's secret strategy |
| Observations | The moves your opponent actually plays |
| Transition probabilities | How likely your opponent is to switch strategies |
| Emission probabilities | How likely each strategy produces the move you just saw |
| Viterbi path | Your best reconstruction of what your opponent was thinking all along |

You are not predicting the future.

You are **reverse-engineering a rational actor from observable behavior**.

---

## 🧠 The Fundamental Problem: Incomplete Information Games

In classical game theory, the landmark framework is **Harsanyi's incomplete information game**.

The setup:

- Your opponent has a type
- You cannot directly observe that type
- You observe actions
- You infer the hidden type and respond optimally

This is exactly the HMM problem.

---

## 🔥 The Deepest Insight

**Filtering is the algorithm for maintaining beliefs; Viterbi is the algorithm for committing to a story.**
