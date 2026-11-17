# GfloorServer (1.21.11)

A performance-focused **fork of [CraftCanvasMC/Canvas](https://github.com/CraftCanvasMC/Canvas)** (`ver/1.21.11`), built for large SMP servers.

Maintained by **sea**, **urboy**, and **xlay**.

## What this fork adds

All changes are tuned to keep vanilla gameplay, PvP, redstone, and farms intact while cutting down tick overhead when players cluster together.

### Region & SMP Optimizations
- **Crowd Region Splitting (`gfloor.enableCrowdRegionSplit`)**: Uses `4x4` chunk region sections (`gridExponent <= 2`) so dense player areas split across separate region threads instead of crowding one thread.
- **Entity Collision Cap (`gfloor.enableCrowdEntityThrottling`)**: Limits per-tick collision checks to `4` when more than 6 entities stack together.
- **Distant Mob AI Throttling (`gfloor.aiThrottleDistance`)**: Reduces full goal/target evaluation frequency for idle mobs beyond `32` blocks in crowded regions while keeping running goals active.
- **Idle Hopper Cooldown Scaling (`gfloor.optimizeHopperUnderLoad`)**: Doubles check cooldown only for hoppers that did not move any items in crowded regions.
- **Dynamic Chunk Random Ticking (`gfloor.enableDynamicChunkTicking`)**: Scales `randomTickSpeed` in crowded regions (`crowdedRandomTickDivisor: 2`).
- **Idle Item & XP Merge Throttling (`gfloor.optimizeItemAndXpMerge`)**: Increases stationary item merge scan interval (`40 -> 60` ticks) and idle XP orb player-scan interval (`1 -> 4` ticks).
- **Empty Effects Skip (`gfloor.skipEmptyEffectsTick`)**: Skips potion effect iteration when an entity has no active effects.
- **Idle Villager Brain Throttle (`gfloor.optimizeIdleVillagerBrain`)**: Halves brain ticks for non-trading villagers in crowded regions.
- **Distant Pathfinding Throttle (`gfloor.throttleDistantPathfinding`)**: Delays path recomputation for mobs without a target in crowded regions.
- **Fast Spectator/Removed Push Skip**: Skips `pushEntities` immediately for removed or spectator entities.
- **In-Ground Arrow Optimization (`gfloor.optimizeInGroundArrows`)**: Skips redundant block collision shape allocations and reduces block-effect checks for arrows already stuck in blocks.
- **Idle Minecart Event Skip (`gfloor.skipUnusedVehicleUpdateEvent`)**: Avoids allocating `Location` and `VehicleUpdateEvent` every tick when no plugins listen to `VehicleUpdateEvent`.
- **Spawner Idle Scan Throttle (`gfloor.optimizeSpawnerIdleScan`)**: Halves player-range scan frequency while `spawnDelay > 20` without changing spawn timing.
- **Beacon Scan Optimization (`gfloor.optimizeBeaconScan`)**: Halves sky/pyramid re-scan frequency for active beacons.
- **Idle Boat Water-Check Skip (`gfloor.optimizeIdleBoats`)**: Skips redundant water-level scans on empty, stationary boats.
- **Fast Thunder Early-Return (`gfloor.fastThunderEarlyReturn`)**: Returns immediately from `tickThunder` when not thundering.
- **Mob Equipment Update Throttle (`gfloor.throttleMobEquipmentUpdates`)**: Checks mob equipment changes every other tick.
- **Crowded Target Search Throttle (`gfloor.throttleCrowdedTargetSearch`)**: Doubles `NearestAttackableTargetGoal` random interval in crowded regions.
- **Idle Furnace Fast-Path (`gfloor.optimizeIdleFurnaces`)**: Skips recipe checks on empty, unlit furnaces/smokers/blast furnaces.
- **Idle BrewingStand Fast-Path (`gfloor.optimizeIdleBrewingStands`)**: Skips potion mix checks on empty, idle brewing stands.
- **Empty Campfire Fast-Path (`gfloor.optimizeEmptyCampfires`)**: Skips cooking loops on empty campfires.
- **Out-of-Bounds Projectile Discard (`gfloor.fastDiscardOutOfBoundsProjectiles`)**: Discards projectiles immediately when far outside world height bounds.
- **Static Marker ArmorStand Throttle (`gfloor.throttleStaticMarkerArmorStands`)**: Reduces tick frequency for static, marker, no-gravity armor stands.
- **Skip Empty Equipment Sync (`gfloor.skipEmptyEquipmentSyncOnPair`)**: Skips redundant mob `detectEquipmentUpdates` after pairing when equipment is empty.

### Enabled Canvas Defaults
- `enableSuffocationOptimization`: `true`
- `enableWorkStealing`: `true`
- `enableMidTickTasks`: `true`
- `fluidPostProcessingToScheduledTick`: `true`
- `optimizeAquifer`: `true`
- `useEndBiomeCache`: `true`
- `optimizeBeardifier`: `true`
- `optimizeNoiseGeneration`: `true`
- `deduplicateShuffledTemplatePoolElementList`: `true`
- `optimizeStructureGen`: `true`
- `filterClientboundSetEntityMotionPacket`: `true`
- `reduceUselessMovePackets`: `true`
- `optimizePlayerListTicking`: `true`
- `asyncProtocolSwitch`: `true`
- `cactusCheckSurvivalBeforeGrowth`: `true`
- `enableCachedMTBEntityTypeConvert`: `true`

## Building

Requires **Java 21**.

```bash
./gradlew applyAllPatches
./gradlew createMojmapPaperclipJar
```

The runnable server JAR is generated in `canvas-server/build/libs/`.

## License

See [LICENSE](LICENSE).
