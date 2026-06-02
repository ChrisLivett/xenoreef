# Alien Fish Exchange Reference Notes

Working research notes for understanding the original Alien Fish Exchange game loop. This is not a reproduction spec. It records observed mechanics and source links for historical/reference purposes.

## Source Status

Primary sources currently checked:

- Wayback capture of the fish guide index, 2003-12-19: https://web.archive.org/web/20031219032012/http://alienfishexchange.com:80/guide/fish.html
- Raw Wayback fish guide capture: https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/guide/fish.html
- FAQ: https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/faq.html
- What-is page: https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/what.html
- How-to-play distribution page: https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/play.html
- Breederboard page: https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/leader/world/
- Sample species pages:
  - https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/guide/red.html
  - https://web.archive.org/web/20031219032012id_/http://alienfishexchange.com:80/guide/acute.html
- Local TV screenshots in `afx/images/`, especially:
  - `tv-image3.gif` and `tv-image4.gif` for tank/dashboard stats.
  - `tv-image5.gif`, `tv-image8.gif`, and `tv-image10.gif` through `tv-image14.gif` for the in-game Breeder's Bible.
  - `tv-image16.gif` and `tv-image17.gif` for market buying.
  - `tv-image18.gif` through `tv-image23.gif` for tank, fish detail, freeze, and freezer screens.

Secondary sources:

- Codex Gamicus says the game was a fish breeding game played through Sky/ntl in the early 2000s, with a tank, buying, breeding, and selling fish: https://gamicus.fandom.com/wiki/Alien_Fish_Exchange
- GameSpot reported in 2000 that nGame games including Alien Fish Exchange were distributed on Sprint PCS Wireless Web: https://www.gamespot.com/articles/ngame-wireless-web-games/1100-2650852/
- Justia lists the ALIEN FISH EXCHANGE word mark as abandoned with status date 2005-09-24: https://trademarks.justia.com/760/65/alien-fish-exchange-76065488.html

## High-Level Concept

Alien Fish Exchange was a persistent asynchronous breeding and trading game for mobile and interactive TV-era platforms. The player controlled a remote tank, raised alien fish, encouraged breeding and mutation, kept fish fed, sold valuable stock, and competed through network/provider leaderboards.

The fiction was explicitly about alien aquatic life discovered on Europa. The market fantasy was culinary: restaurants wanted exotic off-world fish.

## TV Interface Notes

The TV screenshots show a compact 4:3-ish game viewport with a persistent right-hand status/action sidebar. The sidebar includes:

- Alien Fish logo.
- Current cash, displayed with `Ku` currency formatting.
- Context labels such as `Sell fish`, `OK!`, or `Guide`.
- Icon buttons for major actions and screens.

The tank screen shows animated fish in a side-on aquarium scene with rocks/coral. A separate status screen shows summary values:

- Day.
- Fish.
- Schools.
- Cash.
- Value.

One screenshot shows a Day 1 summary with Fish 100, Schools 1, Cash Ku10000, and Value Ku10000. This confirms that the original tracked both individual fish count and school/group count.

## Verified Core Loop

1. Player owns a tank containing fish. The TV interface displays individual fish/counts while also tracking schools as a separate stat.
2. Player buys fish from the Exchange when available.
3. Player manually advances to a new day.
4. Fish act at night: they eat, grow, breed, mutate, or die.
5. Player manages food-chain risk because fish can eat other fish.
6. Player sells fish for currency.
7. Progress is measured by cash, tank value, number of fish, and/or completing the species catalog.

The FAQ confirms a manual "new day" action, and says fish only act at night. It also confirms freezing as a tool for stopping feeding, predation, or vulnerability to predation.

## Player Goals

The FAQ gives two explicit win framings:

- Accumulate the most money.
- Breed all fish types.

The leaderboard page shows competitive ranking by:

- Number of fish.
- Value of tank.
- Cash.

The source index also included provider-specific leaderboards, suggesting the live game was partitioned by mobile or TV distribution partner and aggregated into a world board.

## Time Model

The game appears turn-like rather than real-time. The player initiates a daily tick. During the nightly tick, fish process their actions.

Observed effects during day advance:

- Fish consume prey or starve if food is unavailable.
- Fish grow by species-specific daily growth.
- Fertile fish breed according to species rules.
- Fish mutate on a species-specific day, sometimes with probability or death.

Design implication: the old game's strategy came from deciding when to advance time, which fish to keep active, which to freeze, what food stock to maintain, and when to sell before a mutation or death event.

## Fish Data Model

Individual species pages expose a consistent data structure:

- Notes/flavor.
- Whether the fish can be bought directly.
- Species/pair it can be bred from.
- Species it can mutate from.
- Birth weight.
- Target weight.
- Daily growth.
- Fertility day.
- Mutation day and mutation result.
- Breeding outcomes by partner species.
- Prey list.
- Hunger rate as a percentage of body weight per day.
- Eating style.
- Typical sell price per fish.
- Typical buy price per gram.
- Estimated value per fish.
- Estimated profit per fish.

The TV version exposed the same kind of information through an in-game Breeder's Bible. Screenshots show alphabetical/family tabs such as `A - B`, `C - F`, `G - R`, `S - Z`, and `Help`, plus species-detail tabs:

- Info.
- Source.
- Life Cycle.
- Feeding.
- Value.

Two sampled species demonstrate the range:

- A basic prey species can be tiny, fertile almost immediately, unable to eat other fish, useful as food stock, and low value.
- A higher-value predator can be much heavier, take longer to become fertile, eat a whole prey family, and have mutation outcomes that include death or conversion into another species.

The TV screenshots also show a selected-fish detail screen for a Red Sprat with:

- Count/name: 1 Red Sprat.
- Age: 0 days.
- Weight: 2g.
- Worth: Ku4.
- Action buttons including sell and freeze.

This suggests the player-facing interface dealt with concrete fish or small counted groups, even though the simulation also used a "schools" concept.

## TV Guide Sample: Friendly Grouper

The Friendly Grouper screenshots demonstrate how a full species entry appeared in the TV Breeder's Bible:

- Info tab: short species flavor text and artwork.
- Source tab: buyable from AliFEx, no direct "bred from" entry, and mutation sources including Kant's Philosofish, Manx Catfish, and Ultra-violet Sprat.
- Life Cycle tab: birth weight 3000g, target weight 18000g, daily growth 833g, fertile on Day 10, and mutation on Day 19 with an 80% death chance and 20% Jovian Piranha outcome.
- Breeding section: multiple partner-to-offspring rules are listed on the same page, with the self/preferred row visually emphasized.
- Feeding tab: prey includes all Sprat and all Eel, hunger is 10% of body weight per day, and eating style is any suitable prey.
- Value tab: typical sell price Ku120000, typical buy price Ku15 per gram, estimated value Ku270000, and estimated profit Ku142985.

This confirms the TV version exposed the same strategic data as the website, but in a tabbed in-game UI optimized for remote-control navigation.

## Species Catalog

The archive fish index lists 47 fish split across 11 families:

| Family        | Count | Original examples                                        |
| ------------- | ----: | -------------------------------------------------------- |
| Angelfish     |     3 | Blazing, Coruscating, Iridescent                         |
| Angle fish    |     4 | Acute, BermudaTri, Blasphemous, Right                    |
| Bass          |     3 | Double, Drummen, Walking                                 |
| Catfish       |     3 | Manx, Schrodinger's, Siamese                             |
| Choirfish     |     4 | Heavenly, Wailing, Whistling, Yodelling                  |
| Eel           |     5 | Atomic, Parasitic, Piranha-eating, Recombinant, Swarming |
| Europan Shark |     3 | Greater, Lesser, Pan                                     |
| Grouper       |     6 | Faded, Friendly, Legendary, Rock, Super, Support         |
| Philosofish   |     5 | Kant's, Nietzsche's, Plato's, Russell's, Zen             |
| Piranha       |     4 | Invisible, Ionic, Jovian, Nano                           |
| Sprat         |     7 | Blue, Green, Grey, Rainbow, Red, Ultra-violet, White     |

These names and family concepts are useful only for understanding the original game's structure.

Per-fish detail pages generated from the archived Breeder's Bible species pages live in `fish/`. Each file contains the fish's notes, source data, life cycle, breeding rules, feeding data, and value data.

## Breeding Model

Verified behavior:

- A school becomes fertile on a species-specific day.
- The fertile period runs from the species' fertile day until its mutation day. This rule comes from user memory of the original game and fits the archived species page structure.
- A fertile school generally prefers one breeding partner.
- On the final day of its fertile period it can breed with any possible partner.
- Species pages list breeding outcomes in the form "partner species => offspring species."
- Same-species and cross-species pairings both exist.
- Some fish are not purchasable and must be discovered through breeding or mutation.

The in-game TV guide confirms that breeding recipes were visible inside the game, not only on the website. For example, the Friendly Grouper Life Cycle tab lists its fertility day, mutation day/outcomes, and multiple partner-to-offspring pairings.

## Mutation Model

Verified behavior:

- Many species have a mutation event on a specific day.
- Mutation can transform a fish into another species.
- Mutation can be probabilistic.
- Mutation can result in death.
- Species pages also list what other species can mutate into the current species.

Design implication: mutation is both opportunity and risk. Players may freeze or sell fish to avoid bad mutations, or hold fish to chase rare transformations.

## Feeding And Predation

Verified behavior:

- Hunger is species-specific but commonly expressed as a percentage of body weight per day.
- Prey can be "None," a family, or specific categories.
- Eating style can matter, such as preferring the largest suitable prey.
- Piranha behavior is intentionally chaotic: the FAQ states they may eat themselves.
- Freezing is the main control for preventing a fish from eating, starving, or being eaten.

Design implication: the economy is not just buy low/sell high. Food-web management creates risk, loss, and planning pressure.

The TV guide's Feeding tab for Friendly Grouper shows the same field structure as the web guide: prey list, hunger rate, and eating style.

## Market And Economy

Verified behavior:

- Currency was Kudos.
- Species pages distinguish sell price per fish from buy price per gram.
- Species pages estimate value and profit.
- Buy availability varies by species.
- Leaderboards rank cash and tank value, so liquid money and living inventory both matter.
- The TV market shows current per-fish purchase prices in Kudos.
- The TV buy screen shows the current Exchange price, how many fish the player can afford, a quantity entry field, and total cost.

User recollection:

- Prices fluctuated dynamically.
- The exact formula is unknown, but lower stock levels may have increased prices.

Screenshot examples:

- A Sprat market screen lists current prices for Blue, Green, Grey, Red, and White Sprat.
- A Blue Sprat buy screen shows a current Exchange price of Ku51 each and calculates that the player can afford 195 fish with Ku9950 cash.
- The Red Sprat detail screen shows a separate current worth value for the selected fish/count, suggesting sale value was based on the fish itself and not only the market list price.

Open questions:

- Whether market supply/demand changed by provider or date is not yet verified.
- Exact buy/sell formulas are not yet verified beyond species page typical values.

## Freeze Mechanic

The FAQ frames freezing as a strategic pause state. Use cases:

- Prevent starvation while away.
- Prevent a fish from being eaten.
- Prevent a predator from eating valuable or needed stock.
- Delay breeding or mutation timing.

This makes freezing a major planning lever in the original game's daily rhythm.

The TV screenshots show a distinct freeze flow:

- The selected fish detail screen includes a freeze action.
- A confirmation screen says the player has frozen the selected fish/count.
- A separate freezer-like screen shows frozen fish against an icy white background.
- A frozen fish detail screen labels the fish as frozen and keeps its age, weight, worth, and description visible.

## Multiplayer And Persistence

The game was multiplayer in an asynchronous ranking sense. The site advertised persistent-world mobile play, while leaderboards showed rankings by provider and by world.

## Platform Notes

The archived distribution page lists US, Canada, UK, Europe, and Australia mobile/web partners. Secondary sources and old wiki notes also mention interactive TV via Sky/ntl. The original design was therefore constrained by low-bandwidth, low-input devices, which likely explains the turn-like "new day" loop and compact text-heavy guide.
