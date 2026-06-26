# CardVoyager Phase 2 Exploration And Graphics Closeout

Date: 2026-06-26

## Theme

CardVoyager moved from a Cardlandia Chapter 1 proof-of-concept into the beginning of Phase 2 exploration: launch, Pullaverse flight, Star Map catalog, Sol as first destination, and a graphical Cardlandia/space presentation using staged image assets.

---

## Repository Checkpoint

Repo: `D:\MY AI\Repos\CardboardEmpiresDev`

Branch: `master`

Commit pushed:

`011290c Add CardVoyager phase 2 exploration prototype`

GitHub remote:

`https://github.com/AceWaffles/CardboardEmpiresDev`

Final repo status after push was clean.

---

## Major Design Decisions

- The Pullaverse is a static universe, not procedurally generated.
- Players discover existing systems rather than generating them.
- Cardlandia is fixed at sector `(94,120)`.
- Sol is the first known catalog target at sector `(99,120)`, five sectors east of Cardlandia.
- Cardlandia's inner system is protected home space and not normally explorable.
- Star Map is a Captain's Chair command and full-screen mode, not a minimap.
- Plotting a course does not autopilot the ship; the player manually flies to the destination.
- Fuel is expedition pressure, not punishment.
- Planet identity is permanent, expedition content is temporary, and player knowledge is permanent.
- Story/sidequest/player-owned locations persist; procedural expedition POIs can change between expeditions.

---

## New Game Bible Work

Added:

- `CardboardEmpires.CardVoyager\GameBible\02 - Gameplay\Phase 2 Exploration Architecture.md`
- `CardboardEmpires.CardVoyager\GameBible\01 - World\Systems\Sol.md`

Updated:

- `GameBible\01 - World\Pullaverse.md`
- `GameBible\02 - Gameplay\Exploration.md`
- `GameBible\02 - Gameplay\Ship Systems.md`
- `GameBible\03 - Engine\Save System.md`

Important content captured:

- Five exploration layers: Cardlandia, Pullaverse, Star System, Planetary Orbit, Planet Surface.
- Pullaverse movement uses ship-centered viewport behavior.
- Star Map displays sector-level coordinates with 10-sector guide lines.
- Sol planetary lineup: Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune.
- Gas giants and crushing-gravity/toxic/extreme worlds require special handling or tech before landing.
- Orbit layer should eventually support scan, analysis, landing risk, and landing-zone selection.

---

## Implemented Gameplay/Engine Work

New scenes/modes:

- `pullaverse`
- `starmap`
- `PullaverseMode`
- `StarMapMode`

Departure flow:

- Replaced `BOARD SHIP` placeholder path with `BEGIN LAUNCH`.
- Begin launch transitions from Cardlandia Departure Station into Pullaverse navigation.
- Pilot/Navigator launch quip added.

Pullaverse navigation:

- Ship stays centered.
- WASD/arrow movement scrolls space around the ship.
- Ship rotates toward heading, including diagonal input such as `W+D`.
- Navigation state stores sector, local sector position, heading, background offset, and plotted destination.

Star Map:

- Full-screen Star Map scene added.
- Shows Cardlandia and Sol as current catalog entries.
- Shows current sector and plotted destination state.
- `PLOT COURSE: SOL` stores destination but does not move the ship automatically.
- `RETURN TO BRIDGE` exits Star Map.
- `Escape` exits Star Map.

---

## Graphics Work

Staged assets copied into CardVoyager RCL static assets:

- `wwwroot\Assets\Cardlandia\cardlandia-layer-1.png`
- `wwwroot\Assets\Cardlandia\council-sprite.png`
- `wwwroot\Assets\Characters\captain-avatar-sprite.png`
- `wwwroot\Assets\Ships\starship-avatar.png`
- `wwwroot\Assets\Ships\scout-pointer.svg`
- `wwwroot\Assets\Space\space-region-0.png`

Cardlandia:

- Replaced CSS-art Cardlandia background with staged Cardlandia image.
- Added static Council building image.
- Removed old CSS orbital ring artifact.
- Removed old central crystal artifact.
- Removed coordinate badge under title.
- Removed `GATEWAY TO THE PULLAVERSE` floating caption.
- Hubs moved toward outer wall/cutout positions.
- Council made smaller and positioned near the top wall/cutout.
- Council in-range behavior now shows only a small `COUNCIL` floating label, not a large yellow glow box.
- Cardlandia movement uses an elliptical walkable boundary and faster vertical movement to feel better in 16:9.

Captain avatar:

- Replaced CSS `C` marker with staged astronaut sprite sheet.
- Final mapping assumes 4x4 sheet with rows as directions and columns as animation frames.
- Row order:
  - Row 1: forward/down
  - Row 2: backward/up
  - Row 3: right
  - Row 4: left

Pullaverse:

- Replaced CSS ship marker with staged ship image.
- Added staged space background.
- Removed ship glow/wake effects after visual review.
- Reduced background flicker by removing sky-region swapping.
- Fixed visible seam/jump by stopping modulo wrapping on main bitmap.
- Sharpened background by avoiding global pixel rendering on the space bitmap and reducing aggressive upscale.
- Stardust overlay remains useful and should be preserved.

---

## Verification

Passed before commit:

```powershell
node --check CardboardEmpires.CardVoyager\wwwroot\cardvoyager.js
dotnet build CardboardEmpires.CardVoyager\CardboardEmpires.CardVoyager.csproj /p:UseSharedCompilation=false
```

Full web build command:

```powershell
dotnet build CardboardEmpires.Web\CardboardEmpires.Web.csproj
```

Known intermittent blocker remains:

- IIS Express may lock `CardboardEmpires.Web\bin\Debug\net9.0\CardboardEmpires.CardVoyager.dll`.
- Recent lock process seen: `iisexpress.exe`.
- This is an environment/process lock, not a CardVoyager syntax/build issue.

---

## Next Session Starting Points

Recommended next technical step:

1. Tune Cardlandia hub positioning/hitboxes so visual art and interaction zones match better.
2. Add first static system discovery behavior: manually fly toward Sol and trigger system discovery/entry when within range.
3. Build initial Star System Mode for Sol with star + planetary lineup.
4. Build Planetary Orbit Mode contract before surface gameplay: scan, analyze, landing risk, landing-zone selection.
5. Decide whether all Cardlandia hubs should become art assets or remain styled buttons for now.

Potential graphics cleanup:

- Replace remaining hub buttons with art assets one at a time.
- Keep static images unless sprite sheets are perfectly aligned and divisible by frame grid.
- For animated buildings, prefer separate well-aligned frames or sprite sheets with exact frame dimensions.

---

## Important Asset Notes

- `theCouncil.png` was changed from a failed 3x3 animated sheet to a static transparent image.
- Building-wide sprite sheets can visually slide if frame dimensions are fractional or the building is not anchored identically per frame.
- For future sprite sheets, use exact dimensions divisible by rows/columns and keep the object registered to the same anchor point in every frame.
