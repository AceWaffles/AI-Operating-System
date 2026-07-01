# Decisions Registry

## 2026-06-16

Decision:

Morning Meeting belongs to AI Operating System.

Status:

ACTIVE

Reason:

Synchronizes all AI participants.

---

## 2026-06-16

Decision:

Morning Meetings are immutable.

Status:

ACTIVE

Reason:

Historical context matters.

---

## 2026-06-16

Decision:

Not every idea becomes work.

Status:

ACTIVE

Reason:

Prevents burnout and overcommitment.

---

## 2026-06-16

Decision:

Cardboard Empires wins by reducing collector friction.

Status:

ACTIVE

Reason:

Focus on solving collector problems rather than becoming an image repository.

---

## 2026-06-16

Decision:

Cardboard Empires V1 staging deployment uses RDP, not SMB or admin shares.

Status:

ACTIVE

Reason:

Port 445 was previously found unavailable, and admin shares such as `\\Cardboard_Emp\c$` must not be assumed.

V1 deployment flow is Development PC -> Visual Studio Build -> OpenCode Publish -> Package Deployment -> RDP to Cardboard_Emp -> Execute Deploy Script.

---

## 2026-06-18

Decision:

Cardboard Empires public/static pages share the collector-room visual language until a different approved brand direction exists.

Status:

ACTIVE

Reason:

Keeps Home, About, and Community cohesive while the product is still stabilizing.

---

## 2026-06-18

Decision:

Community is a Coming Soon area, not a live social feature.

Status:

ACTIVE

Reason:

Prevents unfinished Shoutouts/Nominations workflows from presenting as active public product features.

---

## 2026-06-18

Decision:

Binder pages use left `My Empire` navigation, center global search, and reusable right rail widgets.

Status:

ACTIVE

Reason:

Creates an app-like collector workspace and avoids one-off right-side jump menus.

---

## 2026-06-18

Decision:

SetInventory should move toward paged or lazy-loaded lists instead of right rail jump menus for huge checklists.

Status:

ACTIVE

Reason:

Large set checklists need scalable navigation and a cleaner list/table experience.

---

## 2026-07-01

Decision:

Cardboard Empires mobile and desktop experiences serve different jobs.

Status:

ACTIVE

Reason:

Mobile is collector interaction: quick binder/card browsing, 4-card carousel pages, drawers, sheets, one-handed quantity edits, and front/back flipping. Desktop remains collection management: dense tables, power-user controls, and larger-screen organization.

---

## 2026-07-01

Decision:

Production server-local configuration is authoritative for the real production connection string.

Status:

ACTIVE

Reason:

The repository `appsettings.Production.json` remains a non-secret placeholder. Deployment must preserve the server-local Integrated Security connection string or login/authenticated pages can fail with 500 errors.

---

## 2026-07-01

Decision:

Card image upload readiness is proven by DB/local audit before R2 upload.

Status:

ACTIVE

Reason:

`SetCheckItems.FrontImageId` and `BackImageId` define the application URL contract. Local files must match exact expected image-key paths before upload; generic placeholders do not satisfy missing exact paths.
