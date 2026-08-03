# Crucible Threat Calculator

A standalone Foundry VTT module (not a system edit) that adds a live threat meter to the Combat Tracker
when playing the **Crucible** game system.

## What it does

While a Combat encounter is active, a bar appears under the Combat Tracker's round/round header showing:

`Party <N>   [ Rating ]   Adversaries <N>`

- **Party threat** = sum of `Actor#threat` for every Combatant with a friendly-disposition token (Heroes +
  allied NPCs).
- **Adversary threat** = sum of `Actor#threat` for every Combatant with a hostile-disposition token.
- **Rating** = Trivial / Easy / Moderate / Hard / Deadly, based on the ratio of Adversary to Party threat.

It updates automatically as combatants are added/removed, tokens change disposition, or actors change
level/rank.

## Rescaling adversaries (GM only)

Click the rating badge (e.g. "Hard") to open a dropdown of all five difficulty levels. Picking one rescales
every hostile Adversary's **rank** (Minion/Normal/Elite/Boss) and **level** together: for each Adversary,
every rank is paired with the level that best hits its target threat at that rank, and whichever
(rank, level) pair lands closest to the target wins — ties favor the fewest changes from where the
Adversary already is. Level does most of the fine-grained work since it's continuous; rank only shifts
when it clearly improves the fit. A Boss stays relatively tougher than its Minions since each is scaled
proportionally to its own current threat. Rank/level changes also affect that Adversary's action economy,
Heroism cap, and health/morale, per the Crucible system's own rules.

Only visible to GMs — players see nothing, not even a static badge. Rescaling suppresses Crucible's
floating "+Health" combat text, but if an Adversary token's **Display Bars** setting shows bars to everyone
(not just its Owner), players will still see its HP/Morale bar shift when rescaled — Foundry has no way to
hide that at the data level, so set those tokens' bar visibility to "Owner" if you want rescaling to stay
completely invisible mid-fight.

## Install

1. In Foundry's **Add-on Modules** tab, click **Install Module** and paste this manifest URL (if hosted), or
   use **Install from local file** with this folder/zip.
2. Enable **Crucible Threat Calculator** in your world's Manage Modules list.
3. Requires the Crucible system to be active.

## Tuning difficulty thresholds

Edit the `DIFFICULTY_BANDS` array at the top of `scripts/threat-calculator.mjs` — each entry's `ratio` is
the upper bound (exclusive) of Adversary/Party threat for that band to apply.
