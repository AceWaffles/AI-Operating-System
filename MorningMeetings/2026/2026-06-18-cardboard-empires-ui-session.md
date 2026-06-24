# Cardboard Empires UI And Binder Session

Date: 2026-06-18

## Theme

Move Cardboard Empires from a functional staging app toward a cohesive collector-facing product experience while preserving deployment safety and AIOS memory.

---

## Major Outcomes

- The finalized public Home2 design was promoted to the public home route `/`.
- The temporary `/Home2` test page was removed.
- Public static pages were aligned around the shared collector-room visual language.
- `/About` was rebuilt with the collector-room background and floating glass layout.
- `/Community` was added as a Coming Soon product area.
- Header navigation now links Community to `/Community/Index`.
- Live Shoutouts and Nominations UI was removed from the public product surface.
- Binder navigation was redesigned around a `My Empire` left rail.
- Binder global search was moved into the center content column.
- Binder pages now support a reusable right rail widget system.
- Right-rail jump menus were removed from Binder pages for now.
- `/Binders/MySets` was renamed to `/Binders/Sets`.
- `GlobalSearch` was updated to respect authorization and collection universe preferences.
- `SetInventory` filters and view controls were moved into a collapsed `Filters & View` panel.
- `SetInventory` received right rail widgets for set summary and checklist help.
- `CardDetails2` was added as a mockup/preview route for future card detail UX.
- Front/back placeholder card assets and card detail stat icons were added.
- Multiple temp-output solution builds succeeded with `0 Warning(s)` and `0 Error(s)`.

---

## Active Product Direction

Public/static site:

- Use a polished collector-room visual language.
- Keep Home, About, and Community visually consistent.
- Community is a Coming Soon product area, not a live social feature yet.

Binder app:

- Treat Binder as an app-like logged-in collector workspace.
- Use left rail navigation, center global search, and reusable right rail widgets.
- Keep admin-only features role-gated and hidden from normal users.
- Avoid right-side section jump menus for now.
- Prefer reusable widget rails over page-specific one-off navigation.

SetInventory:

- Keep filters collapsed unless the user opens them.
- Move toward a denser dark list/table UX matching the provided mockup.
- Do not add a live image column for now because hover preview can handle image lookup.
- Use pagination or lazy loading for large checklists.
- 25 rows per page is acceptable.
- Preserve existing parallel behavior, but opening one parallel expansion should close the previous expansion.

---

## Key Decisions

- Public/static pages should share the Home collector-room background and layout language until a different approved brand direction exists.
- Community is not ready for live Shoutouts/Nominations and remains Coming Soon.
- Binder pages should use reusable right rail widgets instead of right rail jump menus.
- Binder global search belongs in the center content area, not above the full app shell.
- `/Binders/Sets` is the supported sets route; `/Binders/MySets` should not return.
- `GlobalSearch` must stay authorization-aware and collection-preference-aware.
- `SetInventory` should use pagination/lazy loading instead of jump menus for huge lists.
- Do not log every page view per user; log admin actions, imports, blocked imports, errors, permission denials, slow requests over threshold, and important write actions.
- Future logging should remain compatible with Loki/Grafana, but DB-backed structured logs are acceptable first.

---

## Files Changed In CardboardEmpiresDev

Primary repo:

- `D:\MY AI\Repos\CardboardEmpiresDev`

Public/static pages and assets:

- `CardboardEmpires.Web\Pages\Index.cshtml` - promoted finalized public homepage.
- `CardboardEmpires.Web\Pages\About\Index.cshtml` - rebuilt About page with shared public visual language.
- `CardboardEmpires.Web\Pages\Community\Index.cshtml` - added Community Coming Soon page.
- `CardboardEmpires.Web\Pages\Shared\_SiteHeader.cshtml` - updated Community navigation.
- `CardboardEmpires.Web\wwwroot\Assets\Hero\home2-collector-room.png` - shared public hero/background image.
- `CardboardEmpires.Web\wwwroot\Assets\Hero\home2-collector-room.svg` - retained original fallback/source asset.
- `CardboardEmpires.Web\wwwroot\Assets\Icons\Pillars\collect.svg` - public pillar icon.
- `CardboardEmpires.Web\wwwroot\Assets\Icons\Pillars\organize.svg` - public pillar icon.
- `CardboardEmpires.Web\wwwroot\Assets\Icons\Pillars\discover.svg` - public pillar icon.
- `CardboardEmpires.Web\wwwroot\Assets\Icons\Pillars\community.svg` - public pillar icon.

Removed public live-community/testing UI:

- `CardboardEmpires.Web\Pages\Shoutouts\Index.cshtml`
- `CardboardEmpires.Web\Pages\Shoutouts\Index.cshtml.cs`
- `CardboardEmpires.Web\Pages\Nominations\Nominate.cshtml`
- `CardboardEmpires.Web\Pages\Nominations\Nominate.cshtml.cs`
- `CardboardEmpires.Web\Pages\Nominations\ThankYou.cshtml`
- `CardboardEmpires.Web\Pages\Nominations\ThankYou.cshtml.cs`
- `CardboardEmpires.Web\wwwroot\js\Shoutouts.js`

Binder shell and widgets:

- `CardboardEmpires.Web\Pages\Shared\_BindersLayout.cshtml` - app shell with center search and universal right rail support.
- `CardboardEmpires.Web\Pages\Shared\_BindersSideMenu.cshtml` - `My Empire` left navigation.
- `CardboardEmpires.Web\Pages\Shared\_BinderRightRailWidgets.cshtml` - reusable right rail widget renderer.
- `CardboardEmpires.Web\Models\Binders\BinderRailWidgetVm.cs` - right rail widget model.

Binder pages:

- `CardboardEmpires.Web\Pages\Binders\Index.cshtml` - Binder landing with control cards and widgets.
- `CardboardEmpires.Web\Pages\Binders\Inventory.cshtml` - widget rail and removed jump menu behavior.
- `CardboardEmpires.Web\Pages\Binders\Inventory.cshtml.cs` - summary data support.
- `CardboardEmpires.Web\Pages\Binders\WishList.cshtml` - widget rail and removed jump menu behavior.
- `CardboardEmpires.Web\Pages\Binders\WishList.cshtml.cs` - data shape support for future widgets.
- `CardboardEmpires.Web\Pages\Binders\Sets.cshtml` - renamed supported sets page with wider two-column layout.
- `CardboardEmpires.Web\Pages\Binders\Sets.cshtml.cs` - renamed page model from `MySetsModel` to `SetsModel`.
- `CardboardEmpires.Web\Pages\Binders\SetInventory.cshtml` - collapsed filters and right rail widgets.
- `CardboardEmpires.Web\Pages\Binders\SetInventory.cshtml.cs` - nullable warning fixed and current checklist model retained.
- `CardboardEmpires.Web\Pages\Binders\CardDetails.cshtml` - standard card details page with widget rail.
- `CardboardEmpires.Web\Pages\Binders\CardDetails2.cshtml` - secondary card details mockup/preview.
- `CardboardEmpires.Web\Pages\Binders\GlobalSearch.cshtml` - JSON endpoint remains layout-free.
- `CardboardEmpires.Web\Pages\Binders\GlobalSearch.cshtml.cs` - search now respects authorization and collection preferences.

Card detail assets:

- `CardboardEmpires.Web\wwwroot\Assets\Cards\card-placeholder.svg`
- `CardboardEmpires.Web\wwwroot\Assets\Cards\card-placeholder-back.svg`
- `CardboardEmpires.Web\wwwroot\Assets\Icons\CardDetails\odds.svg`
- `CardboardEmpires.Web\wwwroot\Assets\Icons\CardDetails\points.svg`
- `CardboardEmpires.Web\wwwroot\Assets\Icons\CardDetails\thickness.svg`
- `CardboardEmpires.Web\wwwroot\Assets\Icons\CardDetails\variants.svg`

---

## Build And Verification

Temp-output solution builds succeeded with no warnings or errors.

Command used:

```powershell
dotnet build "D:\MY AI\Repos\CardboardEmpiresDev\CardboardEmpires.slnx" -p:OutDir="D:\MY AI\Repos\CardboardEmpiresDev\temp_build\"
```

Reason for temp output:

- Normal builds can fail when DLLs are locked by `devenv.exe` or `iisexpress.exe`.
- Temp output avoids locked web binaries during verification.

---

## Known Risks And Follow-Ups

- `SetInventory` still needs the mockup-style dense list/table redesign.
- `SetInventory` still needs pagination or lazy loading; it currently server-loads all rows.
- `SetInventory` parallel expansions still need mutually exclusive open behavior.
- Live image column should be avoided for now; hover preview should handle card image lookup.
- Removed Shoutouts/Nominations UI does not remove historical entities, migrations, or tables; that was intentional to avoid schema risk.
- Normal build may still be blocked by Visual Studio or IIS Express file locks.
- Production remains inactive and not deployable.
- A separate legacy documentation file in `CardboardEmpires.com` may contain an old Azure connection string with password and still needs a separate approved cleanup task.

---

## Next Session Startup Notes

Load in this order:

1. `D:\MY AI\AGENTS.md`
2. `D:\MY AI\OpenCodeTooling.md`
3. `AI-Operating-System\Now.md`
4. `AI-Operating-System\Index\projects.md`
5. `AI-Operating-System\Index\decisions.md`
6. This session summary

Recommended next work:

1. Update `SetInventory` list/table look to match the provided mockup.
2. Add 25-per-page pagination or lazy loading to `SetInventory`.
3. Keep existing parallel functionality but close previous expansion when a new parallel expansion opens.
4. Verify with temp-output build and remove `temp_build` afterward if desired.
5. Add page-specific right rail widgets later for Sets, Trade Partners, and Universes.
6. Revisit true checklist upsert import later.
7. Brand `/Error` before public launch.

---

## Current Milestone Fit

This session supports Milestone 2 by improving repository understanding, recording repeatable UI/product decisions, and preserving safe startup context for future OpenCode sessions.
