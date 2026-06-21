# Slack Has Increasing Marginal Returns

**Q:** Why does each additional unit of slack provide more value than the previous unit?

**A:** Because the number of "noisy" opportunities you can exploit scales as ~√n of your slack units, not linearly.

- To handle ~n independent uncertain demands, you only need ~√n slack units (since independent noise sums as √n, not n).
- Going from 0 → 1 unit of slack: handle ~1 extra opportunity.
- Going from 9 → 10 units of slack: handle ~20 extra opportunities.

**Result:** Slack has *superlinear* returns. The marginal value of slack *increases* as you accumulate more of it.

*Source: johnswentworth, LessWrong, 29 Jul 2021*
