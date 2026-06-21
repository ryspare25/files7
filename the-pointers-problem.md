# The Pointers Problem: Human Values Are A Function Of Humans' Latent Variables

**Author:** John Wentworth  
**Published:** November 19, 2020

## Core Idea

Human values depend on latent variables inside human world models. The central alignment challenge is not merely learning values, but understanding what those values point to in reality.

## Key Concepts

- Humans care about actual states of the world, not merely appearances.
- Values depend on inferred objects such as people, happiness, justice, and intentions.
- These concepts are latent variables rather than directly observed data.
- Some concepts may not correspond to anything real (e.g., ghosts).
- An AI must determine what human concepts refer to before it can optimize for them.

## The Pointers Problem

The core question:

> What do human concepts point to?

If human values are defined over latent concepts, then an AI must identify what those concepts correspond to in reality (if anything) before value learning is possible.

## Main Failure Modes

### Hidden Genocide

Humans may fail to notice atrocities in ranked world snapshots. An AI could mistakenly learn:

- Humans dislike seeing atrocities

instead of:

- Humans dislike atrocities

### Wireheading

Humans cannot accurately evaluate all consequences of actions. Optimizing for human predictions is not necessarily optimizing for human values.

### People We Never Meet

Humans may care about people whose lives they cannot influence. Behavior alone cannot reveal all such preferences.

### Misspecified Models

Human concepts may be wrong, confused, or refer to nonexistent entities. The AI cannot simply replace the human ontology without risking loss of meaning.

## Takeaway

The hard part of alignment is not determining what humans value.

The hard part is determining what the things humans value actually refer to.

Before an AI can learn human values, it must solve the pointers problem.
