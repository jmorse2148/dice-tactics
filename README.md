# Dice Tactics

A 2-player tactical board game where dice ARE the pieces. Inspired by the
"choose your weapon" D&D dice t-shirt — every die type (d4 through d12) is
viable, they just win in different ways. A d20 sits on each player's throne
as their life total.

**Play it here:** https://jmorse2148.github.io/dice-tactics/

Right now it's a local two-player prototype — both players share one
screen and take turns. Online play (open a link, join with a session code,
play against a friend remotely) is in progress.

## What's in this repo

- `index.html` — the playable prototype. Open it directly in a browser, or
  play the hosted version at the link above.
- `design-doc.md` — the living design document: rules, balancing
  decisions, and open questions, updated as they get resolved during
  playtesting.

## Status

Core loop is playable: movement, combat, HP, win condition, the throne
piece (d20) can move but never attacks. Healing is designed but not yet
implemented in the prototype — it's being held back until online play is
working, so it can be built against synced game state from the start
rather than retrofitted later.
