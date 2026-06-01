# XenoReef Game Design Document

Draft 0.1. This GDD uses Alien Fish Exchange only as structural inspiration. XenoReef should feel like a modern strategic idle aquarium game with its own fiction, creatures, economy, UI, and progression.

## Pitch

XenoReef is a mobile idle-strategy aquarium game where players manage a compact alien reef lab, breed strange aquatic organisms, balance a living food web, discover mutations, and sell or preserve specimens to grow their operation.

The hook is simple: every "reef day" matters. Before advancing time or closing the app, the player decides what to feed, freeze, pair, sell, or protect. The tank then resolves overnight: creatures eat, grow, breed, mutate, or die.

## Target Platform

- Primary: Android.
- Engine: Unity.
- Session length: 1-5 minutes for routine check-ins, 10-20 minutes for planning and collection sessions.
- Monetization assumption for the first design: premium or free prototype with no monetization. Avoid pay-to-wait and avoid mechanics that punish real-life absence too harshly.

## Design Pillars

1. Strategic idle, not passive idle.
2. Small tank, deep consequences.
3. Discoverable biology.
4. Cozy but slightly uncanny.
5. Offline-first, fair, and readable.

## Player Fantasy

The player runs an independent xenobiology reef station. They are not just flipping fish for profit; they are curating living systems, meeting research contracts, restoring damaged reef biomes, and deciding which discoveries are worth risk.

This keeps the buy-breed-sell loop but changes the emotional frame from old-school exchange market to modern creature strategy and collection.

## Core Loop

1. Review tank state: active species, hunger, fertility, mutation timers, threats, and contract opportunities.
2. Make planning choices: buy stock, assign breeding pairs, freeze specimens, separate predators, sell surplus, reserve food species.
3. Advance a reef day manually or let an offline day resolve when the player returns.
4. Watch a concise event report: births, deaths, growth, meals, mutations, sales, completed contracts.
5. Spend rewards on tank capacity, stasis slots, scanners, biome modules, or new starter stock.
6. Update the collection journal and choose the next breeding target.

## What Stays Close To The Original

| Area | Preserve |
| --- | --- |
| Time | Discrete day/tick resolution. |
| Fish actions | Creatures act during the tick: eat, grow, breed, mutate, die. |
| Economy | Buy common stock, breed higher-value stock, sell for currency. |
| Discovery | Some species cannot be bought and must be bred or mutated. |
| Risk | Predators can eat useful creatures; starvation matters. |
| Planning | Freezing/stasis prevents eating, being eaten, starvation, and mutation timing. |
| Goals | Wealth, tank value, and complete collection are all valid progress paths. |

## Where XenoReef Diverges

| Area | XenoReef Direction |
| --- | --- |
| Fiction | Reef lab, research, restoration, and contract ecology rather than Europan gourmet fish trade. |
| Names | Fully original organism names and taxonomy. |
| Art | Modern stylized bioluminescent reef; no reuse of old fish silhouettes, icons, or UI. |
| Data | New formulas, values, growth rates, breeding graph, and mutation tree. |
| UI | Touch-first Android interface with event logs, filters, warnings, and batch actions. |
| Multiplayer | Single-player first; seasonal leaderboards later. |
| Tone | Cozy science-fantasy with gentle humor rather than copying the original joke taxonomy. |

## Main Resources

- Credits: spendable currency from contracts and specimen sales.
- BioMass: food and growth resource, produced by feeder species and consumed by predators.
- Research Data: meta-progression earned by discovering species, recording mutations, and completing contracts.
- Stasis Charges: limited daily or crafted capacity to freeze organisms.
- Tank Value: total estimated value of live organisms and installed modules.

## Time And Offline Model

XenoReef should support two time modes:

- Manual day: player taps `Advance Day` to resolve one day immediately.
- Offline catch-up: when returning after time away, the game offers to resolve up to a capped number of days.

Recommended MVP rule:

- One real-world hour equals one available reef day, capped at 8 stored reef days.
- Player can preview risk before resolving multiple days.
- If the projected outcome includes preventable starvation or predation, show warnings and allow adjustments.

This keeps the old deliberate day-advance rhythm while making it comfortable on Android.

## Creature Data Model

Each organism species has:

- Species ID and display name.
- Family/ecotype.
- Rarity tier.
- Buy availability.
- Birth mass.
- Adult/target mass.
- Daily growth.
- Fertility start day.
- Fertility end day.
- Preferred breeding partner.
- Backup breeding partners.
- Breeding outputs.
- Mutation day.
- Mutation outcomes with probabilities.
- Hunger rate.
- Prey rules.
- Eating style.
- Base sell value.
- Buy cost formula.
- Habitat requirements.
- Journal clues.

Each individual school/colony has:

- Species ID.
- Count.
- Average mass.
- Age.
- Frozen/stasis flag.
- Fertile flag.
- Hunger state.
- Habitat assignment.
- Discovered lineage metadata.

## Feeding Model

Every active organism consumes food during day resolution. Food can come from:

- Smaller prey organisms in the same habitat.
- Stored BioMass.
- Passive feeder colonies.
- Manual feeding before day advance.

Eating rules should be explicit and previewable. Examples:

- Grazer: consumes BioMass only.
- Filterer: produces small BioMass if not overcrowded.
- Hunter: eats the largest suitable prey school.
- Swarm: eats many tiny prey.
- Cannibal: may consume its own school if starving.
- Symbiote: reduces another species' hunger if cohabiting.

## Breeding Model

Breeding happens during day resolution.

Rules:

- A school becomes fertile at a species-specific age.
- By default, the fertile window lasts from fertility start until that species' mutation day.
- Most species prefer one partner.
- During the last fertile day, a species may accept any valid partner.
- Breeding consumes energy or BioMass.
- Breeding produces a juvenile school of the output species.
- Breeding can fail if the tank is overcrowded, habitat conditions are wrong, or required food is missing.

MVP simplification:

- Use school-level breeding rather than tracking individual organisms.
- Allow one breeding event per fertile school per day.
- Show "possible offspring" as hidden silhouettes until discovered.

## Mutation Model

Mutation is a timed risk/reward event.

Rules:

- Species can have a mutation check at a specific age.
- Outcomes may include transformation, split into multiple species, sterility, death, or rare upgraded variant.
- The player can freeze a school to delay mutation.
- Research upgrades can reveal probabilities.

Example XenoReef mutation patterns:

- Glowcap Drifter: day 4, 70% becomes Prism Drifter, 30% remains stable.
- Knifeleaf Fry: day 6, 60% becomes Bladegrazer, 20% becomes Glassgrazer, 20% dies.
- Hollow Ray: day 9, mutates only if kept in a dark habitat.

## Stasis / Freeze Mechanic

Stasis is a core strategic tool.

Effects:

- Frozen organisms do not eat.
- Frozen organisms cannot be eaten.
- Frozen organisms do not grow.
- Frozen organisms do not breed.
- Frozen organisms do not mutate.

Limits:

- Early player has 2 stasis slots.
- Upgrades add slots or reduce unfreeze cooldown.
- Some contracts require live active specimens, preventing stasis abuse.

## Economy

Players earn Credits through:

- Selling surplus organisms.
- Completing research contracts.
- Completing conservation requests.
- Discovering first-time species.
- Maintaining target tank value milestones.

Costs:

- Buying starter species.
- Habitat modules.
- Stasis expansion.
- Food reserves.
- Scanners and journal upgrades.
- Tank expansion.

Suggested MVP economy:

- Common feeder species are cheap and low profit.
- Mid-tier predators are profitable but risky.
- Rare species are discovered through breeding/mutation and are usually not directly buyable.
- Sales create short-term cash but may set back discovery chains.
- Market prices should fluctuate from typical values using a simple stock-sensitive modifier, where scarce species become more expensive to buy and abundant species become less valuable to sell.

## Progression

Phase 1: Starter Reef

- 8-10 species.
- One habitat.
- Basic buying, selling, day advance, feeding, breeding, mutation, and stasis.

Phase 2: Habitat Strategy

- Add warm vent, dark trench, crystal shoal, and open-water modules.
- Species gain habitat preferences and mutation conditions.

Phase 3: Research Contracts

- Add rotating objectives such as "breed three stable filterers" or "deliver a predator above 2kg."
- Contracts gently teach breeding chains.

Phase 4: Seasonal Or Optional Online

- Optional leaderboards for tank value, species discovered, and challenge contracts.
- No direct player-to-player trading in MVP.

## MVP Species Direction

Use original structure, not original content.

Target MVP:

- 12 species across 4 ecotypes.
- 4 buyable starter species.
- 5 breed-only species.
- 3 mutation-only species.
- 2 predator chains.
- 1 chaotic/cannibal species.
- 1 high-value risky species with death mutation.

Example XenoReef ecotypes:

- Drifters: small feeder organisms and early breeding chain.
- Grazers: BioMass consumers and producers.
- Lures: mutation-heavy luminous species.
- Hunters: predatory value species.

Example MVP species names:

- Bluewake Drifter
- Emberwake Drifter
- Palewake Drifter
- Prism Drifter
- Mossjaw Grazer
- Glassjaw Grazer
- Threadfin Lure
- Lantern Lure
- Knifeleaf Hunter
- Hollow Ray
- Silt Crown
- Riftmaw

## Android UI

Primary screens:

- Tank: visual aquarium, alerts, quick actions.
- Ledger: all active schools with age, hunger, value, fertility, mutation timer, and stasis state.
- Market: buy, sell, and contract offers.
- Breeding: pair planner and discovered lineage graph.
- Journal: species catalog, clues, discovered outcomes.
- Lab: upgrades, habitats, stasis slots.
- Day Report: compact log after each resolved day.

Critical UI affordances:

- Risk preview before day advance.
- Clear icons for hunger, fertile, predator, mutation soon, and frozen.
- Batch freeze/unfreeze.
- Filter by family, habitat, risk, value, and age.
- Tap a creature to see "will eat," "can breed with," and "mutation due."

## Day Resolution Order

Proposed deterministic order:

1. Unfreeze any scheduled releases.
2. Apply habitat effects.
3. Consume food / prey.
4. Apply starvation damage or death.
5. Grow surviving active schools.
6. Resolve breeding.
7. Resolve mutation.
8. Update values, contracts, and journal.
9. Produce day report.

The exact order should be visible in rules help because it matters strategically.

## First Prototype Scope

Build the smallest version that proves the loop:

- One tank.
- One habitat.
- 8 species.
- One prey family.
- One predator family.
- Manual day advance.
- Offline day accrual capped at 3 days.
- Buy/sell market.
- Stasis slots.
- Breeding output table.
- Mutation table.
- Day report.
- Collection journal.

No account system, no online leaderboard, no ads, no IAP, no complex animation pipeline.

## Open Design Questions

- Should offline days auto-resolve or require confirmation? Recommendation: require confirmation.
- Should species be represented as individual creatures or schools? Recommendation: schools for MVP.
- Should contracts replace pure selling as the main income source? Recommendation: both, with contracts teaching strategy.
- How complex should dynamic pricing be? Recommendation: start with a transparent stock-based modifier and avoid hidden market simulation until the core loop is fun.
- How punishing should starvation be while the player is away? Recommendation: cap offline resolution and warn before applying severe loss.
- Should stasis be free but slot-limited, or consume charges? Recommendation: slot-limited first.

## Next Documentation Step

Create a mechanical spec with:

- Initial 8-species data table.
- Breeding graph.
- Mutation graph.
- Day-resolution pseudocode.
- MVP Unity scene list.
- Save-game schema.
- First playable milestone checklist.
