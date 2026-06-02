# XenoReef Unity Getting Started Guide

Draft 0.1. This document identifies the Unity scenes, folders, assets, prefabs, scripts, and first implementation milestones needed to start building XenoReef.

It assumes the first playable is based on:

- Android as the primary platform.
- Unity as the engine.
- Manual day advancement.
- One starter reef tank.
- Individual fish, not schools.
- The first 8 species from [xenoreef-initial-fish.md](xenoreef-initial-fish.md).
- A Blender-led 3D-first art pipeline, with sprites/flipbooks allowed as a fallback.

## First Playable Goal

The first Unity build should prove the loop, not the final presentation.

Target experience:

1. Load into a splash/loading screen.
2. Enter the reef tank.
3. Buy starter fish.
4. View individual fish in the tank and Ledger.
5. Advance one reef day manually.
6. Resolve feeding, growth, breeding, mutation, stasis, market value, and contracts.
7. Show a Day Report explaining what happened.
8. Continue playing from the newly saved day.

The first playable does not need final art, full animation, polished audio, online features, ads, IAP, or every species.

## Unity Project Setup

Recommended starting choices:

- Use an LTS Unity editor version rather than a beta or tech-stream editor.
- Start with a 3D project using URP if available and comfortable.
- Target Android early, even while testing mostly in the editor.
- Use portrait orientation first.
- Use a reference UI resolution such as `1080 x 1920`.
- Enable visible `.meta` files and force text serialization.
- Put large binary assets under Git LFS once the Unity project exists.
- Keep AI concept art in documentation or concept folders, not as final production assets.

Rendering direction:

- Use a 2.5D aquarium view: 3D fish swimming in a framed tank, with UI layered over it.
- Use an orthographic or lightly perspective camera for readability.
- Keep fish paths constrained so taps, warnings, and identity remain readable on mobile.
- Use simple placeholder fish first: colored spheres, capsules, flat cards, or low-poly Blender models.

## Recommended Unity Folder Structure

Create the project folders under `Assets/XenoReef/`.

```text
Assets/
  XenoReef/
    Art/
      Concept/
      Models/
        Fish/
        Tank/
        Props/
      Materials/
        Fish/
        Tank/
        UI/
      Sprites/
        FishPortraits/
        Icons/
        UI/
      Textures/
      VFX/
    Audio/
      Music/
      SFX/
      UI/
    Data/
      Species/
      Breeding/
      Mutation/
      Economy/
      Contracts/
      Research/
      Achievements/
      StartingState/
    Prefabs/
      Core/
      Fish/
      Tank/
      UI/
      VFX/
      Debug/
    Scenes/
      Main/
      Dev/
    Scripts/
      Core/
      Data/
      Simulation/
      Economy/
      Contracts/
      Save/
      UI/
      Visuals/
      Input/
      Audio/
      DevTools/
    ScriptableObjects/
    Settings/
    Tests/
      EditMode/
      PlayMode/
```

Avoid a large `Resources/` folder for the MVP. Prefer ScriptableObjects, serialized scene references, and simple catalog assets. Addressables can wait until asset volume or loading requirements justify them.

## Scene Plan

Keep the scene count small at first. Most gameplay panels can be UI prefabs inside the main tank scene rather than separate Unity scenes.

| Scene | Purpose | MVP Need |
| --- | --- | --- |
| `00_Boot.unity` | Starts the app, creates persistent systems, loads save data, opens the loading scene. | Required |
| `01_Loading.unity` | Shows splash art, loading progress, and async scene transition. | Required |
| `02_ReefTank.unity` | Main playable scene: tank view, fish visuals, UI panels, day advancement. | Required |
| `Dev_FishSandbox.unity` | Tests fish prefabs, swim paths, materials, animation loops, tap selection. | Recommended |
| `Dev_SimulationSandbox.unity` | Runs day-resolution tests without needing full UI. | Recommended |
| `Dev_ArtReview.unity` | Reviews fish models, portraits, lighting, scale, and materials. | Optional |

### `00_Boot.unity`

Main objects:

- `GameRoot`
- `Bootstrapper`
- `SceneLoader`
- `SaveManager`
- `GameClock`
- `CatalogDatabase`
- `AudioManager`
- `SettingsManager`

Responsibilities:

- Initialize services once.
- Load or create the player save.
- Load static catalogs such as species, breeding, mutation, economy, and contracts.
- Transition to `01_Loading.unity`, then `02_ReefTank.unity`.

For the earliest prototype, `00_Boot.unity` and `01_Loading.unity` can be combined. Keep them separate once loading art, save validation, and first-run setup become meaningful.

### `01_Loading.unity`

Main objects:

- `LoadingScreenCanvas`
- `SplashPosterImage`
- `LoadingSpinner`
- `LoadingProgressBar`
- `LoadingTipText`

Placeholder assets:

- Use [concept-art/xenoreef-splash-poster.png](concept-art/xenoreef-splash-poster.png) as loading splash reference or temporary art.
- Add the XenoReef logo later using a deterministic Unity UI text or image asset rather than generated image text.

Responsibilities:

- Display splash/poster art while the main scene loads.
- Hide loading details behind a calm, polished presentation.
- Optionally rotate short gameplay tips once the player has enough concepts to learn.

### `02_ReefTank.unity`

Main objects:

- `ReefTankRoot`
- `TankCamera`
- `TankEnvironment`
- `WaterVolume`
- `FishSwimBounds`
- `FishSpawnPoints`
- `FishVisualManager`
- `SelectionManager`
- `SimulationController`
- `DayAdvanceController`
- `MainHudCanvas`
- `PanelStack`
- `DayReportModal`

Primary UI panels in this scene:

- `TankHud`
- `LedgerPanel`
- `MarketPanel`
- `BreedingPanel`
- `JournalPanel`
- `LabPanel`
- `ContractsPanel`
- `AchievementsPanel`
- `SettingsPanel`
- `DayReportModal`
- `ConfirmStartNewDayModal`

Responsibilities:

- Show the active tank and fish.
- Let the player select individual fish.
- Let the player buy, sell, freeze, inspect, and plan.
- Preview risk before starting a new day.
- Run or request day resolution.
- Show day results.
- Save when the new day starts.

## Required Data Assets

Use ScriptableObjects for static design data. Keep runtime player state separate from these definitions.

### `SpeciesDefinition`

One asset per species.

Fields:

- `speciesId`
- `displayName`
- `ecotype`
- `rarity`
- `marketStatus`
- `buyAvailability`
- `birthMass`
- `targetMass`
- `dailyGrowth`
- `fertilityStartDay`
- `fertilityEndDay`
- `mutationDay`
- `hungerRate`
- `preyRules`
- `eatingStyle`
- `baseBuyPrice`
- `baseSellPrice`
- `contractValueModifier`
- `habitatRequirements`
- `journalNotes`
- `visualPrefab`
- `portraitSprite`
- `iconSprite`

Create MVP assets for:

1. `buttonfin_drifter`
2. `peppermint_skipper`
3. `whistlebelly_grazer`
4. `grinfin_nipper`
5. `ribbonback_skimmer`
6. `sockeye_lantern`
7. `prism_lantern`
8. `glassjaw_dandy`

### `BreedingRule`

Fields:

- `parentA`
- `parentB`
- `offspringSpecies`
- `offspringCountMin`
- `offspringCountMax`
- `biomassCost`
- `requiresLastFertileDay`
- `discoveryState`

### `MutationRule`

Fields:

- `sourceSpecies`
- `triggerAge`
- `outcomes`
- `requiresHabitat`
- `canBeDelayedByStasis`
- `journalRevealTier`

Each outcome should include:

- `resultType`: transform, remain, split, soft-loss, BioMass, value-loss.
- `targetSpecies`
- `probability`
- `biomassProduced`
- `eventText`

### `MarketConfig`

Fields:

- `startingCredits`
- `startingBioMass`
- `startingResearchData`
- `priceVariationMin`
- `priceVariationMax`
- `supplyDemandWeight`
- `sellPriceFloor`
- `sellPriceCeiling`
- `collectorFeeRate`

### `ContractDefinition`

Fields:

- `contractId`
- `requestedSpecies`
- `minimumMass`
- `requiredCondition`
- `deadlineDays`
- `payout`
- `reputationReward`
- `researchReward`
- `riskTag`
- `offerWeight`

### `StartingStateDefinition`

Fields:

- `startingDay`
- `startingCredits`
- `startingBioMass`
- `startingStasisSlots`
- `startingSpeciesUnlocked`
- `starterMarketListings`
- `starterContracts`
- `initialFish`

## Runtime Save Data

Keep save data plain and serializable. Do not save direct scene object references.

Suggested top-level save structure:

```text
SaveGame
  version
  currentDay
  credits
  biomass
  researchData
  collectorReputation
  stasisSlotCount
  creatures[]
  discoveredSpecies[]
  discoveredBreedingRules[]
  discoveredMutationRules[]
  activeContracts[]
  completedAchievements[]
  marketState
  settings
```

Each creature instance should store:

- `creatureId`
- `speciesId`
- `age`
- `mass`
- `isFrozen`
- `hungerState`
- `lastEatenDay`
- `parents`
- `lineageTags`
- `habitatId`
- `condition`
- `hasResolvedMutation`

## Core Prefabs

### App/Core Prefabs

- `GameRoot.prefab`
- `SceneLoader.prefab`
- `AudioManager.prefab`
- `CatalogDatabase.prefab`
- `SaveManager.prefab`

### Tank Prefabs

- `ReefTankRoot.prefab`
- `TankEnvironment_Starter.prefab`
- `WaterVolume_Starter.prefab`
- `FishSwimBounds.prefab`
- `FishSpawnPoint.prefab`
- `SelectionRing.prefab`
- `TapRipple.prefab`

### Fish Prefabs

Start with a generic fish prefab and specialize from there.

- `FishVisual_Base.prefab`
- `FishVisual_ButtonfinDrifter.prefab`
- `FishVisual_PeppermintSkipper.prefab`
- `FishVisual_WhistlebellyGrazer.prefab`
- `FishVisual_GrinfinNipper.prefab`
- `FishVisual_RibbonbackSkimmer.prefab`
- `FishVisual_SockeyeLantern.prefab`
- `FishVisual_PrismLantern.prefab`
- `FishVisual_GlassjawDandy.prefab`

Base fish prefab components:

- `FishVisual`
- `FishSwimmer`
- `FishAnimator`
- `FishSelectionTarget`
- `FishStatePresenter`
- `Collider` or `Collider2D` for taps
- Mesh or sprite renderer
- Optional particle emitters for bubbles, glow, or mutation-ready state

### UI Prefabs

- `MainHudCanvas.prefab`
- `ResourceStrip.prefab`
- `AlertStack.prefab`
- `BottomNav.prefab`
- `TankHud.prefab`
- `CreatureCard.prefab`
- `LedgerPanel.prefab`
- `MarketListingCard.prefab`
- `MarketPanel.prefab`
- `ContractCard.prefab`
- `BreedingPairSlot.prefab`
- `BreedingPanel.prefab`
- `JournalSpeciesEntry.prefab`
- `JournalPanel.prefab`
- `LabPanel.prefab`
- `DayReportModal.prefab`
- `DayReportEventRow.prefab`
- `ConfirmStartNewDayModal.prefab`
- `Tooltip.prefab`

## Core Script Areas

Keep the simulation mostly independent from Unity scene objects. The game should be able to resolve a day in edit-mode tests without loading the tank scene.

### Core

- `GameBootstrapper`
- `ServiceRegistry`
- `SceneLoader`
- `GameClock`
- `GameEvents`

### Data

- `SpeciesDefinition`
- `BreedingRuleDefinition`
- `MutationRuleDefinition`
- `MarketConfig`
- `ContractDefinition`
- `StartingStateDefinition`
- `CatalogDatabase`

### Simulation

- `CreatureInstance`
- `TankState`
- `DayResolutionInput`
- `DayResolutionResult`
- `FeedingSystem`
- `GrowthSystem`
- `BreedingSystem`
- `MutationSystem`
- `StasisSystem`
- `DiscoverySystem`
- `DayResolver`
- `RiskPreviewService`

### Economy And Contracts

- `MarketService`
- `PricingService`
- `SupplyDemandState`
- `ContractService`
- `ContractGenerator`
- `SaleService`
- `TankValueCalculator`

### Save

- `SaveGame`
- `SaveManager`
- `SaveSerializer`
- `SaveVersionMigrator`
- `AutosaveService`

### UI

- `MainHudController`
- `ResourceStripController`
- `PanelStackController`
- `LedgerPanelController`
- `MarketPanelController`
- `BreedingPanelController`
- `JournalPanelController`
- `LabPanelController`
- `DayReportController`
- `RiskWarningController`
- `CreatureCardPresenter`

### Visuals

- `FishVisual`
- `FishVisualManager`
- `FishSwimmer`
- `FishAnimator`
- `FishSelectionTarget`
- `FishStatePresenter`
- `TankCameraController`
- `TankEnvironmentController`
- `VfxSpawner`

## First 8 Species Asset Checklist

For each first-playable species, create:

- `SpeciesDefinition` asset.
- Placeholder fish visual prefab.
- Portrait/icon placeholder.
- Basic material or color palette.
- Simple swim parameters.
- Ledger row/icon mapping.
- Journal entry stub.
- Market listing if buyable.
- Breeding rules where relevant.
- Mutation rule where relevant.

| Species | Need To Buy? | Needs Breeding Rule? | Needs Mutation Rule? | Needs Predator Logic? |
| --- | --- | --- | --- | --- |
| Buttonfin Drifter | Yes | Yes | Yes | No |
| Peppermint Skipper | Yes | Yes | Yes | No |
| Whistlebelly Grazer | Yes | Yes | Yes | No |
| Grinfin Nipper | Yes | Yes | Yes | Yes |
| Ribbonback Skimmer | No | Yes | Yes | No |
| Sockeye Lantern | No | Yes | Yes | Yes |
| Prism Lantern | No | No | Yes | No |
| Glassjaw Dandy | No | Yes | Yes | Yes |

## Minimum Art Asset List

Use temporary assets until the loop is fun.

Required for the first playable:

- Splash/loading image.
- Starter tank background or simple 3D tank environment.
- 8 placeholder fish visuals.
- 8 fish portrait/icon placeholders.
- Resource icons: Credits, BioMass, Research Data, Collector Reputation, Stasis.
- Status icons: hunger, fertile, mutation soon, frozen, predator, contract.
- UI panel background, button, tab, card, modal, progress bar.
- Basic VFX: bubbles, selection ring, mutation flash, sale sparkle, stasis frost.

Nice-to-have:

- Ambient water particles.
- Fish glow materials for Lantern species.
- Simple tank props: moon-base window, reef modules, coral clusters, pipes/tubes.
- Loading spinner styled as a small orbital scanner.
- Short UI sound effects.

## Minimum Audio Asset List

For the earliest build, audio can be placeholders.

Required later for feel:

- Gentle ambient tank loop.
- Button tap.
- Panel open/close.
- Buy/sell confirmation.
- Start new day.
- Day report reveal.
- Fish selected.
- Stasis freeze/unfreeze.
- Mutation event.
- Warning alert.

## Implementation Order

### Milestone 1: Empty App Flow

Goal: prove Unity scenes and navigation.

- Create `00_Boot.unity`.
- Create `01_Loading.unity`.
- Create `02_ReefTank.unity`.
- Load from boot to loading to tank.
- Show placeholder splash art.
- Set Android portrait orientation.
- Add a simple main HUD with a disabled `Start New Day` button.

### Milestone 2: Static Data And Save

Goal: prove the game can load species and create a starting save.

- Create ScriptableObject definitions for the first 8 species.
- Create starting state data.
- Create save data classes.
- Create save/load/reset flow.
- Show day, credits, BioMass, and stasis slots in the HUD.

### Milestone 3: Tank And Fish Placeholders

Goal: see individual creatures in the tank.

- Add starter tank environment.
- Add generic fish visual prefab.
- Spawn fish from save data.
- Add simple swim movement.
- Add tap selection.
- Show selected fish summary.

### Milestone 4: Ledger And Market

Goal: manage fish as individual economic objects.

- Build Ledger panel with creature rows.
- Build Market panel with starter species listings.
- Implement buy fish.
- Implement sell fish.
- Update credits, tank value, and active creatures.

### Milestone 5: Day Resolution

Goal: prove the core game loop.

- Implement `DayResolver`.
- Resolve feeding.
- Resolve growth.
- Resolve breeding.
- Resolve mutation.
- Resolve stasis.
- Generate a `DayResolutionResult`.
- Save at the start of the new day.
- Show Day Report.

### Milestone 6: Strategy Preview

Goal: make the day advance feel fair.

- Add risk preview before starting a new day.
- Warn about hunger, valuable prey, mutation next day, last fertile day, and contract deadline.
- Let the player cancel and adjust the tank.

### Milestone 7: Contracts And Journal

Goal: add modern progression hooks.

- Add rotating collector contracts.
- Add basic contract completion.
- Add Journal entries.
- Reveal discovered species, breeding results, and mutation results.

### Milestone 8: First Art Pass

Goal: replace pure placeholders with a coherent visual direction.

- Build simple Blender models for the first 4 buyable species.
- Add basic swim/turn/eat animation loops.
- Add fish portraits rendered from the same models.
- Add improved tank lighting and water.
- Keep performance checks on Android in the loop.

## Testing Checklist

Start with edit-mode tests for the simulation.

Core tests:

- A day advances by exactly one.
- Frozen fish do not eat, grow, breed, mutate, or get eaten.
- Hungry fish consume valid prey according to prey rules.
- Fish grow by the correct amount.
- Fertile fish breed only within the valid fertility window.
- Last fertile day fallback partner logic works.
- Mutation triggers on the correct age.
- Mutation probabilities resolve into valid outcomes.
- Sell price and tank value update after growth/mutation.
- Day Report includes causes for major events.
- Save/load preserves creature IDs and state.

Play-mode tests:

- Boot loads into the tank.
- Buying a fish spawns a visible fish.
- Selling a fish removes the correct fish.
- Tapping a fish selects the correct creature.
- Start New Day shows a report and updates the tank.
- Opening and closing panels does not break selection.

## Early Performance Targets

Use these as practical guardrails, not final requirements.

- Keep the first tank under 30 visible fish.
- Keep fish rigs simple.
- Avoid expensive transparent materials until tested on Android.
- Limit full-screen post-processing.
- Pool fish visuals, bubbles, and common UI rows.
- Test on a real Android device early.

## Common Traps To Avoid

- Do not build every screen before the day loop works.
- Do not hand-place every fish as a scene object; spawn from save data.
- Do not mix static species data with runtime creature state.
- Do not make the first tank visually noisy before selection and warnings are readable.
- Do not create a complex animation pipeline before the first 8-species loop is fun.
- Do not rely on generated image text for logos, labels, or UI.
- Do not lock into real-time 3D if Android performance or art consistency becomes painful.

## First Unity Session Checklist

Use this as the practical starting list.

1. Create the Unity project.
2. Create the `Assets/XenoReef/` folder structure.
3. Add the three main scenes: `00_Boot`, `01_Loading`, `02_ReefTank`.
4. Add the splash poster as a temporary loading image.
5. Create a `GameRoot` prefab.
6. Create first versions of `SpeciesDefinition`, `CreatureInstance`, and `SaveGame`.
7. Create `SpeciesDefinition` assets for Buttonfin Drifter and Peppermint Skipper.
8. Create one generic fish visual prefab.
9. Spawn two fish in the tank from a hardcoded starting state.
10. Add a button that advances the day and increments age.
11. Save and load that tiny state.

Once those work, expand to the full first 8 species and day-resolution systems.
