# XenoReef Game Design Document

Draft 0.2. XenoReef should feel like a modern strategic manual-day aquarium economy game with its own fiction, creatures, market, UI, and progression.

## Pitch

XenoReef is a mobile aquarium strategy game where players manage an artificial alien reef hidden on a moon, breed strange aquatic organisms, balance a living food web, discover mutations, and trade rare specimens through a secret off-world collectors' exchange.

The hook is simple: every "reef day" matters. The player decides what to buy, feed, freeze, pair, sell, or protect, then manually starts the next day. The reef resolves: creatures eat, grow, breed, mutate, or are lost, and the player decides whether to keep playing or stop after the new day begins.

## Target Platform

- Primary: Android.
- Engine: Unity.
- Session length: 1-5 minutes for routine check-ins, 10-20 minutes for planning and collection sessions.
- Initial time model: manual day advancement only; no waiting required.
- Monetization assumption for the first design: premium or free prototype with no monetization. Avoid pay-to-wait and avoid mechanics that punish real-life absence too harshly.

## Design Pillars

1. Strategic day advancement, not passive waiting.
2. Small tank, deep consequences.
3. Discoverable biology.
4. Cozy, odd, and lightly spooky.
5. Manual-first, fair, and readable.

## Player Fantasy

The player runs an artificial reef station concealed on a remote moon. The station looks like a research habitat from the outside, but its real business is breeding unusual aquatic life for collectors, brokers, and off-world pet keepers.

The fantasy is entrepreneurial, secretive, and playful: buy common stock, breed or mutate rarer organisms, decide which specimens to keep for future lines, and sell high-value creatures before the market shifts. Research, achievements, and contracts add modern progression without replacing the core money-making loop.

## Audience And Tone

XenoReef should be all-ages strange-and-funny. It can feel secretive, surprising, and a little spooky, but it should not feel bleak, cruel, or adult-only.

Tone guardrails:

- Strange first, spooky second. Even rare creatures should have a comic hook, toy-like silhouette, or surprising charm.
- The collectors' exchange is secret because the fish are rare and impractical, not because the player is doing anything mean-spirited.
- Predation and specimen loss are strategic risks, but present them with non-graphic aquarium language: vanished, shed, poofed into BioMass, bonked itself, escaped into the filter, or became unstable.
- Avoid gore, realistic suffering, cruelty, and mean-spirited contracts.
- Keep late-game species bigger, stranger, and more mysterious through shape language, behavior, sound, and animation rather than harsh imagery.
- UI copy should sound like a playful lab ledger mixed with a collectors' catalog.

Current visual direction references:

- [All-ages fish concept sheet](concept-art/xenoreef-all-ages-fish-sheet.png)
- [All-ages UI mood mockup](concept-art/xenoreef-all-ages-ui-mood.png)
- [Balanced UI mood mockup](concept-art/xenoreef-balanced-ui-mood.png)
- [Splash poster concept](concept-art/xenoreef-splash-poster.png)

## Core Loop

1. Review tank state: individual creatures, hunger, fertility, mutation timers, threats, market prices, and contract opportunities.
2. Make planning choices: buy stock, assign breeding pairs, freeze specimens, separate predators, sell surplus, and reserve food species.
3. Start a new reef day manually. The game saves at the start of the new day.
4. Watch a concise event report: births, losses, growth, meals, mutations, sales, completed contracts, research unlocks, and achievements.
5. Spend rewards on tank capacity, stasis slots, scanners, biome modules, collector access, or new starter stock.
6. Decide whether to continue into another day or stop with the new day safely saved.

## Reference Boundary

Original-game research lives in `../afx/docs/original-afx-reference.md`. This GDD should remain focused on XenoReef.

Preserve as abstract mechanics:

- Manual day advancement.
- Creatures eat, grow, breed, mutate, and can be lost during day resolution.
- Buy common stock, breed higher-value stock, and sell for currency.
- Some species cannot be bought directly and must be discovered.
- Predation, starvation, stasis, catalog completion, wealth, and tank value all matter.

Replace as XenoReef expression:

- New name, setting, currency, creature taxonomy, art, UI, prose, formulas, and progression.
- Artificial moon reef and secret collectors' exchange.
- Touch-first Android interface with event logs, filters, warnings, achievements, and contracts.
- All-ages strange-and-funny science fantasy with spooky collectible edges.

## Design Lessons From AFX

The original game's depth came from how simple daily actions interacted with a dense species graph. XenoReef should use these lessons without copying original fish, names, values, or setting.

- Make the in-game Journal/Lab central. Species entries should expose source, life cycle, feeding, breeding, mutation, and value data as the player discovers or researches it.
- Build a real breeding/mutation graph with loops, dead ends, sterile species, mutation-only species, cross-ecotype recipes, and risky rare outcomes.
- Treat mutation timing as economic pressure. A creature nearing mutation should force a choice: sell, freeze, breed, hold for a rare result, or accept possible loss.
- Use varied feeding styles. Predators should differ by prey family and eating style, such as largest prey, smallest prey, any suitable prey, same-species trouble, or swarm feeding.
- Separate market price from specimen worth. Buying, open-market selling, individual specimen value, contract payout, and tank value can all be related but distinct.
- Include restricted collector species. Some rare creatures should be impossible to buy directly and tradeable only through brokers, contracts, or reputation unlocks.
- Preserve the final-fertile-day twist. Most creatures prefer one partner during fertility, but accept any valid partner on the last fertile day.
- Explain every day result. The player should understand why a creature ate, starved, bred, failed to breed, mutated, was lost, completed a contract, or missed a deadline.
- Let achievements act as breadcrumbs. Achievements should reward discovery and quietly teach profitable patterns, not just decorate progress.
- Define MVP species by mechanical role, not only by count.

## Main Resources

- Credits: spendable currency from collector sales and contracts.
- BioMass: food and growth resource, produced by feeder species and consumed by predators.
- Research Data: meta-progression earned by discovering species, recording mutations, and completing contracts.
- Collector Reputation: progression with brokers and collectors, unlocking rarer stock and higher-risk contracts.
- Stasis Slots: limited freezing capacity; each slot can hold one creature.
- Tank Value: total estimated value of live organisms and installed modules.

## Time And Save Model

XenoReef starts with manual day advancement only.

- The player performs actions during the current day.
- The player taps `Start New Day` to save and resolve the next day.
- After the day report, the new day is active and safely saved.
- The player can keep playing immediately or stop there.

Deferred option:

- Offline day accrual may be considered later, but it is out of scope for the first prototype.

This keeps the old deliberate day-advance rhythm while removing waiting pressure from the initial Android design.

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
- Open-market sell formula.
- Contract value modifier.
- Listed/unlisted market status.
- Research visibility tier.
- Habitat requirements.
- Journal clues.

Each individual creature has:

- Species ID.
- Creature ID.
- Mass.
- Age.
- Frozen/stasis flag.
- Fertile flag.
- Hunger state.
- Last eaten day.
- Lineage/parents.
- Contract eligibility flags.
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
- Hunter: eats the largest suitable prey creature.
- Swarm: eats many tiny prey.
- Self-risk: may lose value, shed BioMass, or bother the same species if starving.
- Symbiote: reduces another species' hunger if cohabiting.

Prey rules should support both broad and precise cases:

- Any creature below a mass threshold.
- A specific ecotype.
- A specific species.
- Same-species/self-risk when starving.
- Stored BioMass fallback if no live prey is available.

## Breeding Model

Breeding happens during day resolution.

Rules:

- An individual creature becomes fertile at a species-specific age.
- By default, the fertile window lasts from fertility start until that species' mutation day.
- Most species prefer one partner.
- During the last fertile day, a species may accept any valid partner.
- Breeding consumes energy or BioMass.
- Breeding produces one or more juvenile creatures of the output species.
- Breeding can fail if the tank is overcrowded, habitat conditions are wrong, or required food is missing.
- Some species are sterile, mutation-only, or contract-only discoveries.

MVP simplification:

- Track individual creatures rather than schools.
- Allow one breeding event per fertile creature per day.
- Show "possible offspring" as hidden silhouettes until discovered.
- Show a clear "last fertile day" warning in the Ledger and Day Report.

## Mutation Model

Mutation is a timed risk/reward event.

Rules:

- Species can have a mutation check at a specific age.
- Outcomes may include transformation, split into multiple species, sterility, soft loss, or rare upgraded variant.
- The player can freeze a creature to delay mutation.
- Research upgrades can reveal probabilities.
- Mutation outcomes can be individual or clutch-level, such as one rare specimen or multiple juveniles.
- The Day Report must show the trigger age and outcome reason.

Example XenoReef mutation patterns:

- Sockeye Lantern: day 5, 100% becomes Prism Lantern.
- Prism Lantern: day 4, 50% remains stable, 25% becomes Moonveil Glider, 25% becomes Ribbonback Skimmer.
- Hushpuppet Eel: day 8, 40% becomes Giggle-in-the-Dark, 40% remains stable, 20% sheds into BioMass.

Mutation design targets:

- Early species should teach predictable transformation.
- Mid-tier species should introduce odds and soft loss risk.
- Rare species should include high-value but unstable outcomes.
- At least one MVP species should be valuable specifically because it is hard to keep alive long enough to sell or breed.

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

- Selling surplus organisms through the regular collectors' exchange.
- Completing collector contracts for specific fish.
- Completing research contracts that unlock new knowledge or tools.
- Discovering first-time species.
- Maintaining target tank value milestones.

Costs:

- Buying starter species.
- Habitat modules.
- Stasis expansion.
- Food reserves.
- Scanners and journal upgrades.
- Tank expansion.
- Collector listing fees or deposits for high-value contracts.

Suggested MVP economy:

- Common feeder species are cheap and low profit.
- Mid-tier predators are profitable but risky.
- Rare species are discovered through breeding/mutation and are usually not directly buyable.
- Sales create short-term cash but may set back discovery chains.
- Market prices should fluctuate from typical values using a modest supply-and-demand modifier, where scarce species become more expensive to buy and abundant species become less valuable to sell.
- Variation should be meaningful enough to create timing decisions, but not so large that the game becomes a market spreadsheet.
- Time-limited contracts can offer higher prices for specific species, creating risk/reward decisions around whether to chase a deadline or keep breeding toward longer-term goals.

Value model:

- Buy Price: current exchange price for listed species.
- Open-Market Sell Price: baseline sale value for any sellable creature.
- Specimen Worth: value of a specific creature based on species, age, mass, rarity, and condition.
- Contract Payout: premium paid for a requested species or trait before a deadline.
- Tank Value: total specimen worth plus installed modules.

Market status:

- Listed: can be bought and sold normally.
- Unlisted: cannot be bought; can be traded only through brokers or special contracts.
- Research Restricted: cannot be sold until the player records or unlocks the species.
- Restricted: high payout, higher listing fees, quarantine requirements, and possible contract expiry penalties.

## Progression

Phase 1: Starter Reef

- 8-10 species.
- One habitat.
- Basic buying, selling, day advance, feeding, breeding, mutation, and stasis.

Phase 2: Habitat Strategy

- Add warm vent, shadow trench, crystal shoal, and open-water modules.
- Species gain habitat preferences and mutation conditions.

Phase 3: Research Contracts

- Add rotating objectives such as "deliver a predator above 2kg," "supply a live bonded pair," or "record a mutation."
- Contracts include time-limited collector offers and research tasks.
- Research unlocks better scanners, visible probabilities, new market tiers, and habitat tools.
- Achievements reward milestones such as first mutation, first rare sale, first complete breeding chain, and high tank value.
- Achievements should also suggest next steps, such as surviving a mutation, using stasis to preserve a breeding window, or completing a predator chain.

Phase 4: Seasonal Or Optional Online

- Optional leaderboards for tank value, species discovered, and challenge contracts.
- No direct player-to-player trading in MVP.

## MVP Species Direction

Use the reference structure, not reference content.

The draft initial roster lives in `xenoreef-initial-fish.md`.

Target MVP:

- 12 species across 4 ecotypes.
- 4 buyable starter species.
- 5 breed-only species.
- 3 mutation-only species.
- 2 predator chains.
- 1 chaotic self-risk species.
- 1 high-value risky species with a soft-loss mutation.

MVP species role matrix:

| Role | Purpose | Example XenoReef Direction |
| --- | --- | --- |
| Starter feeder | Teaches buying, growth, and food supply. | Cheap drifter with no prey and low value. |
| Feeder variant | Teaches simple breeding pair changes. | Alternate drifter color/strain with different hunger. |
| Stable breeder | Teaches same-species breeding and steady profit. | Grazer that breeds reliably but sells modestly. |
| Early predator | Teaches prey rules and food-chain risk. | Hunter that eats smallest drifters. |
| Risky predator | Teaches high hunger and sale timing. | Hunter that becomes profitable only after several days. |
| Mutation tutorial | Teaches predictable mutation. | Lure that always transforms on a fixed day. |
| Probabilistic mutation | Teaches odds, research, and stasis timing. | Lure with rare useful outcome and soft-loss risk. |
| Sterile rare | Teaches mutation-only discovery and contract value. | Beautiful pet species that cannot breed. |
| Bridge species | Connects early and mid-tier breeding graph. | Creature needed for multiple recipes. |
| Restricted collector species | Teaches collector sales and reputation. | Cannot be bought; appears only through mutation. |
| Chaotic self-risk species | Teaches containment and freezing. | Eats broad prey or loses value if starving. |
| High-value risky species | Teaches endgame timing in miniature. | Rare creature with huge sale value before likely disappearance or BioMass collapse. |

MVP graph targets:

- At least 4 species buyable on day one.
- At least 3 species discovered only by breeding.
- At least 3 species discovered only by mutation.
- At least 2 sterile species.
- At least 2 species with soft-loss mutation outcomes.
- At least 1 loop where a mutation can return the player to an earlier species.
- At least 1 contract chain that requires keeping a low-value species instead of selling it.

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

## Visual Production Strategy

Recommended direction: 3D-first, 2D-presented.

XenoReef should use the AI concept art as mood and creature-design reference while the game loop is being proven, not as a locked final asset source. The production path should take advantage of Blender experience by building stylized fish as simple 3D models, then deciding whether to show them in Unity as real-time 3D or render them into 2D sprites/flipbooks.

Prototype approach:

- Use AI concept sheets and rough placeholder sprites to test the loop quickly.
- Static fish drawings are acceptable for the earliest prototype, especially if layered with simple bobbing, blinking, squash/stretch, bubbles, and tap reactions.
- Avoid spending production time on final art before the economy, breeding graph, and day-resolution loop are fun.

Preferred MVP approach:

- Build each important fish as a stylized low-poly or mid-poly Blender model.
- Keep silhouettes exaggerated and readable from mobile distance.
- Use simple rigs: body sway, tail bones, fin bones, eye blink, and optional mouth/jaw controls.
- Give each fish a small animation set: idle swim, turn, eat, alert/hungry, fertile/display, mutation reaction, and sale/exit.
- Use material variants for rarity, mutation variants, age, health, and collector value rather than fully separate meshes wherever possible.
- Present the tank as a mostly 2.5D aquarium view, with fish swimming in depth but framed for touch readability.

Rendering options:

- Real-time 3D in Unity is the preferred target if performance and art direction hold up. It supports depth, growth scaling, tap reactions, feeding behavior, and natural turns.
- Blender-rendered sprites or flipbooks are the fallback if real-time 3D becomes too expensive, inconsistent, or hard to polish on Android.
- A hybrid is acceptable: real-time 3D for the tank, rendered portraits/icons for the Ledger, Market, Journal, and contracts.

Final-art path:

- If the prototype proves the game is worth investing in, hire an artist or animator to polish silhouettes, color keys, textures, UI treatment, animation timing, and creature personality.
- Commissioned final art should preserve the all-ages strange-and-funny direction while reducing noise and improving consistency.
- Production assets should be original, commissioned, or self-made; AI images remain concept references unless later legal and platform decisions explicitly allow otherwise.

## Android UI

Primary screens:

- Tank: visual aquarium, alerts, quick actions.
- Ledger: all active creatures with age, hunger, value, fertility, mutation timer, and stasis state.
- Market: buy, sell, and contract offers.
- Breeding: pair planner and discovered lineage graph.
- Journal: species catalog, clues, discovered outcomes.
- Lab: upgrades, habitats, stasis slots.
- Achievements: milestone rewards and collection progress.
- Day Report: compact log after each resolved day.

Critical UI affordances:

- Risk preview before day advance.
- Clear icons for hunger, fertile, predator, mutation soon, and frozen.
- Batch freeze/unfreeze.
- Filter by family, habitat, risk, value, and age.
- Tap a creature to see "will eat," "can breed with," and "mutation due."
- Show separate values for current market price, specimen worth, and contract premium where relevant.
- Highlight unlisted/restricted species and explain how they can be traded.
- Show warnings for "last fertile day," "mutation next day," "contract deadline next day," and "predator will eat valuable prey."

Journal reveal states:

- Unknown: silhouette/name hidden.
- Seen: name and visual unlocked after first encounter.
- Recorded: basic lifecycle and value known after capture or sale.
- Researched: breeding, feeding, and mutation probabilities visible.
- Mastered: full graph links and contract preferences visible.

## Day Resolution Order

Proposed deterministic order:

1. Unfreeze any scheduled releases.
2. Apply habitat effects.
3. Consume food / prey.
4. Apply starvation loss or BioMass collapse.
5. Grow surviving active creatures.
6. Resolve breeding.
7. Resolve mutation.
8. Update values, contracts, and journal.
9. Produce day report.

The exact order should be visible in rules help because it matters strategically.

Day Report requirements:

- Group events by losses, feeding, growth, breeding, mutation, market/contract, research, and achievements.
- For each event, show enough cause to support planning: prey chosen, hunger failure, breeding partner, mutation odds if known, and expired contract deadlines.
- Mark surprises as new discoveries and add them to the Journal.

## First Prototype Scope

Build the smallest version that proves the loop:

- One tank.
- One habitat.
- 8 species.
- One prey family.
- One predator family.
- One sterile mutation-only species.
- One unlisted collector-only species.
- Manual day advance.
- No offline day accrual.
- Buy/sell market.
- Time-limited contracts.
- Research unlocks.
- Achievements.
- Stasis slots.
- Breeding output table.
- Mutation table.
- Day report.
- Collection journal.

No account system, no online leaderboard, no ads, no IAP, no complex animation pipeline.

## Current Design Decisions

- Days are manually triggered; there is no waiting or automatic offline resolution in the first prototype.
- The game saves as each new day starts. The player acts, starts a new day, reviews results, then can continue or stop.
- Species are represented as individual creatures, not schools.
- Selling and contracts both matter. Contracts add targeted risk/reward opportunities without replacing regular sales.
- Dynamic pricing uses modest supply-and-demand variation.
- Starvation while away is not a concern in the first prototype because days do not advance without player action.
- Stasis is slot-limited.
- Fish art should start with AI references and placeholders, then move toward a Blender-led 3D-first pipeline. Static drawings are prototype-only; simple animated fish should be the MVP target.

## Next Documentation Step

The Unity setup guide lives in [xenoreef-unity-getting-started.md](xenoreef-unity-getting-started.md).

Create a mechanical spec with:

- Initial 8-species data table based on `xenoreef-initial-fish.md`.
- Breeding graph.
- Mutation graph.
- Day-resolution pseudocode.
- Save-game schema.
- First playable milestone checklist.
