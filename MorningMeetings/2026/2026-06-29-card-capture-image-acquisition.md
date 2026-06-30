# Card Capture Image Acquisition Prototype Handoff

Date: 2026-06-29

## Theme

Cardboard Capture moved from concept into a working local image-acquisition prototype using a printable capture sheet, OpenCV, and the mounted Insta360/camera rail setup.

---

## Repository Work

Repo:

`D:\MY AI\Repos\CardboardEmpiresDev`

New/updated project:

`CardboardEmpires.CardCapture`

Commits pushed during cleanup/session:

- `e4744a1 Add card capture pipeline scaffold`
- `7662c00 Add OpenCV camera capture proof`
- `5b9e560 Add capture sheet crop extraction`

Repo was clean after the previous cleanup before this handoff documentation update.

---

## Printable Capture Sheets

Generated printable files under:

`D:\MY AI\Deployable\Card-Imaging-Mat\`

Final handoff PDF for testing:

`D:\MY AI\Lobby\Outbox\card-imaging-sheet-letter-horizontal-3x2.pdf`

Design direction:

- US Letter paper.
- 3 rows x 2 columns.
- Horizontal card placement.
- Four page fiducials.
- Asymmetric black slot boxes for orientation.
- Thickened corner anchors.
- Visible margin around card safe area.

---

## What Worked

- OpenCV installed and builds in the `.NET 9` capture project.
- Camera capture works through camera index `0` when the Insta360 app is closed.
- Captures save to `D:\MY AI\Lobby\Outbox`.
- A real capture was produced at `1920x1080`.
- The printed sheet is visible and all four page fiducials can be kept in frame.
- Initial page-boundary detection and perspective correction worked enough to create output files.
- The `process` command produced six files: `A1.jpg`, `A2.jpg`, `B1.jpg`, `B2.jpg`, `C1.jpg`, and `C2.jpg`.

---

## What Failed Or Needs Correction

- The six crop images are not production quality.
- Fixed template-coordinate cropping caused card aspect ratio distortion when the sheet was sideways in the camera frame.
- Some crops were offset or stretched.
- Chrome/foil glare remains a major image-quality problem.
- Current output proves pipeline plumbing only, not final scan quality.

---

## Next Session Starting Point

Recommended next technical step:

1. Preserve raw camera captures as permanent source files.
2. Use page fiducials/corners only to normalize the page coordinate system.
3. Detect each individual slot rectangle or corner anchors after page correction.
4. Perspective-transform each slot/card independently.
5. Export to a fixed card ratio such as `1400x1000` for horizontal cards.
6. Add quality scoring for glare, blur, border visibility, and crop confidence.
7. Add OCR/Tesseract only after card geometry is reliable.

Architecture note:

Camera acquisition should stay local on Windows for now because direct camera access through Docker can be painful. Once file-based processing is stable, it can become an MCP/Docker AI Hotel service that reads from `Lobby\Inbox` and writes to `Lobby\Outbox`.
