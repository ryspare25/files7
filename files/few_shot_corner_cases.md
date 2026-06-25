# Few-Shot Prompting: Handling Corner Cases with Examples

## Core Problem

Naive sentiment classifiers fail on sarcasm. Instructions alone ("watch out for sarcasm") are weaker than a demonstrated example with the correct label.

## The Pattern

```xml
Here is an example input with an ideal response:
<sample_input>Great game tonight!</sample_input>
<ideal_output>Positive</ideal_output>

Be especially careful with tweets that contain sarcasm. For example:
<sample_input>Oh yeah, I really needed a flight delay tonight! Excellent!</sample_input>
<ideal_output>Negative</ideal_output>
```

## What Each Element Does

| Element | Function |
|---|---|
| Positive anchor | Establishes baseline classification behavior |
| Sarcasm example | Directly demonstrates the failure mode with correct label |
| Explicit callout before example | Primes attention; more effective than a note after |
| XML tags (`<sample_input>`, `<ideal_output>`) | Disambiguates example content from instruction content |

## Why XML Wrapping Matters

Without tags, the model has to infer where the example input ends and the expected output begins. Tags make the structure unambiguous — Claude pattern-matches on demonstrated behavior rather than interpreting implicit formatting.

## General Principle

**Examples > instructions for edge cases.**

The more counterintuitive the corner case, the more essential the concrete example. Abstract warnings ("be careful with X") activate less reliably than a labeled demonstration of X with the correct output.

## Prompt Structure Template

```
[Task description]

[Positive/baseline example]
<sample_input>...</sample_input>
<ideal_output>...</ideal_output>

[Warning + corner case label]
<sample_input>...</sample_input>
<ideal_output>...</ideal_output>

[Actual input]
<input_[var]>...</input_[var]>
```
