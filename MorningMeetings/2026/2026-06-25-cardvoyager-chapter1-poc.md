# CardVoyager Chapter 1 POC And Design Bible Handoff

Date: 2026-06-25

## Theme

CardVoyager moved from a StarVoyager prototype into a Cardlandia-focused Chapter 1 proof-of-concept with a state-driven browser game architecture, active Game Bible, and initial crew/ship domain model.

---

## Current State

- CardVoyager is a Razor Class Library inside `CardboardEmpiresDev`.
- Web route: `/CardVoyager`.
- Project path: `D:\MY AI\Repos\CardboardEmpiresDev\CardboardEmpires.CardVoyager`.
- Web app references `CardboardEmpires.CardVoyager` instead of `CardboardEmpires.StarVoyager`.
- `CardboardEmpires.slnx` includes `CardboardEmpires.CardVoyager` instead of `CardboardEmpires.StarVoyager`.
- The StarVoyager project was removed and replaced by CardVoyager.

---

## Gameplay POC

Current playable loop:

1. Start in Cardlandia.
2. Move the Captain with WASD or arrow keys.
3. Approach hubs.
4. Active hub updates from hitbox proximity.
5. Press Enter in Explore Mode to enter a hub.
6. Hub Mode disables movement and routes actions through the Command Module.
7. Escape or `EXIT` returns to Cardlandia Explore Mode.

Implemented Cardlandia hubs:

- Council of Travel
- Recruiting
- Shipyard
- Markets
- Tavern
- Departure Station

Chapter 1 intro loop implemented in localStorage-backed state:

- Council briefing confirms Captain commission and navigation approval.
- Shipyard can register ship and prepare rover.
- Recruiting can hire first crew.
- Markets can purchase initial supplies.
- Departure can review launch status.
- `BOARD SHIP` appears only when requirements are complete.
- Rover Bay acts as the Chapter 1 transition placeholder.

---

## Engine Architecture

The JavaScript prototype was refactored around managers and mode strategy objects.

Core managers:

- `GameStateManager`
- `SceneManager`
- `InputManager`
- `SaveManager`
- `CommandManager`
- `RenderManager`

Current modes:

- `ExploreMode`
- `HubMode`

Rules established:

- `gameState` is the single source of truth.
- Nothing directly changes UI outside render functions.
- Systems change `gameState`.
- Render functions read `gameState` and update modules.
- Commands are data: label, visibleWhen, enabled, action.
- Input is delegated through `game.modes[game.state.mode]`.
- Only Explore Mode moves the avatar.
- Hub Mode owns keyboard behavior differently.

---

## Persistence

Phase 1 uses localStorage.

Current save key:

`cardVoyager.save.v1`

Design rules:

- Browser close preserves campaign state.
- Reloading does not rewind consequences.
- No manual previous saves.
- No save scumming.
- Captain death will eventually end the current campaign and start a new Captain.
- Previous campaigns may leave legacy unlocks through Cardlandia, Academy, and discovered knowledge.

---

## UI State

Current UI modules:

- Viewport
- Status panel
- Command panel
- Log panel
- Portrait panel
- Collapsible controls side panel
- Collapsible objectives side panel
- Debug panel

Recent UI decisions:

- Cardlandia title is outside the viewport.
- Coordinate readout is centered under the title.
- Side panels dock next to the game shell and expand outward.
- HUD meters were removed for now.
- Cardlandia default command module focuses on city commands, not Captain's Chair ship controls.

Known UI note:

- The current visual art is CSS placeholder art.
- Future scenes should migrate to PNG, WebP, sprite sheets, or equivalent browser-friendly image assets with light animation.

---

## Game Bible

Created active design bible under:

`CardboardEmpires.CardVoyager\GameBible`

Sections:

- `00 - Vision`
- `01 - World`
- `02 - Gameplay`
- `03 - Engine`
- `04 - Story`
- `05 - Future`
- `99 - Scratchpad`

Important locked principles:

- CardVoyager is a standalone adventure RPG, not a gamified collection tracker.
- The Captain is the player; the crew are the characters.
- Cardlandia is home.
- The Pullaverse is bigger than the player.
- Curiosity is rewarded.
- Failure creates new possibilities.
- Every mechanic should support exploration, discovery, mystery, or adventure.

---

## Crew And Ship Domain Model

Added initial C# domain model under:

`CardboardEmpires.CardVoyager\Domain`

Crew model includes:

- Name
- Species
- Background
- Ship assignment
- Skills
- Certifications
- Known perks
- Known flaws
- Hidden potential
- Morale
- Health
- Biography

Current species:

- Human
- Feralon
- Archivist
- Thrakk

Current backgrounds:

- Courier
- Academy Graduate
- Mechanic
- Security Cadet
- Research Assistant
- Merchant
- Surveyor

Current skills:

- Piloting
- Navigation
- Engineering
- Medicine
- Science
- Combat
- Communications
- Diplomacy
- Survival

Ship model includes:

- Ship definition
- Player ship
- Ship class
- Availability
- Stations
- Modules
- Advantages
- Disadvantages

Chapter 1 shipyard catalog:

- Scout Class: available.
- Explorer Class: locked preview.
- Cruiser Class: locked preview.

Starter applicant catalog includes six hand-authored recruits:

- Kesh
- Vesk
- Lira
- Torren
- Mina
- Orrin

---

## Verification

Repeatedly passing:

```powershell
node --check CardboardEmpires.CardVoyager\wwwroot\cardvoyager.js
dotnet build CardboardEmpires.CardVoyager\CardboardEmpires.CardVoyager.csproj /p:UseSharedCompilation=false
```

Full web build can pass when IIS Express is not locking the copied CardVoyager DLL:

```powershell
dotnet build CardboardEmpires.Web\CardboardEmpires.Web.csproj
```

Known intermittent blocker:

- `iisexpress.exe` may lock `CardboardEmpires.Web\bin\Debug\net9.0\CardboardEmpires.CardVoyager.dll`.
- Stop IIS Express/Visual Studio before full web build if copy errors occur.

---

## Untracked Generated Artifacts To Avoid Staging

Generated deployment/publish files exist in the CardboardEmpiresDev worktree and should remain uncommitted unless explicitly needed:

- `deployment_package_work/`
- `deployment_packages/CardboardEmpires-Staging-20260622-175059.zip`
- `publish_out/`

---

## Next Recommended Work

1. Finish remaining UI polish only if it blocks usability.
2. Wire the C# crew/ship domain model into the live CardVoyager POC.
3. Build the Recruiting screen around six applicants and four crew slots.
4. Build the Shipyard screen around Scout Class plus locked Explorer/Cruiser previews.
5. Keep localStorage as Phase 1 persistence until the POC proves the systems.
