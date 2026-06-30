# Cardboard Capture

Status:

Sapling

Owner:

Michael

Purpose:

Mobile companion application for Cardboard Empires.

Primary design rule:

Michael the Collector first.

The capture workflow must first solve Michael's own collection digitization problem. If it succeeds there, evaluate whether the workflow is useful to other collectors.

Goals:

- Scan cards
- Quick search
- Wishlist management
- Duplicate tracking
- Convention mode
- Collection reference
- Rapid bulk card digitization
- Automated card cropping
- Perspective correction
- Searchable card backs
- Optional public image library contribution

Cardboard Empires Imaging Standard:

Future capture should standardize the image layout before asking AI to interpret the card. The preferred direction is a predictable capture surface that Cardboard Empires can process automatically.

Supported capture concepts:

- Premium neoprene imaging mat with ArUco markers, camera calibration guides, fixed card slots, color calibration strip, version identifier, and high-contrast surface.
- Printable free PDF template with the same slot geometry, marker locations, and crop coordinates.
- Future scanner cassette, camera stand, dealer imaging station, mobile tray, or other hardware that follows the same geometry.

Processing pipeline:

Raw upload -> perspective correction -> quality scoring -> card cropping -> image normalization -> OCR -> AI metadata extraction -> review -> storage -> collection update.

Quality scoring:

- Sharpness
- Exposure
- Perspective
- Crop confidence
- OCR confidence
- Glare detection

Storage philosophy:

- Always preserve the original uploaded image.
- Derived images are disposable.
- Original captures may be reprocessed later as OCR, AI, and image processing improve.

Community image library:

- Users may optionally contribute processed images to a public Cardboard Empires image library.
- Contribution must be clearly explained during upload.
- Low-quality submissions remain private.

Dependencies:

- Mature Cardboard Empires database
- Cloudflare R2 or equivalent object storage
- Background processing worker path
- Image processing pipeline using tools such as OpenCV, ArUco detection, Tesseract OCR, or AI vision models

Not In Scope:

- Perfect AI identification
- Full website replacement
- Immediate marketplace automation
- Immediate dealer tooling

Possible future services:

- Collection digitization
- OCR processing
- Artist extraction
- Flavor text extraction
- Automated eBay listings
- Marketplace publishing
- Inventory synchronization

Phase:

Future development

Success metric:

A collector can digitize an entire collection with minimal manual effort while producing high-quality, searchable digital records that support the broader Cardboard Empires ecosystem.

---

## 2026-06-29 Image Acquisition Prototype Notes

Status:

Prototype started in `D:\MY AI\Repos\CardboardEmpiresDev\CardboardEmpires.CardCapture`.

What was completed:

- Created a dedicated `.NET 9` console project: `CardboardEmpires.CardCapture`.
- Added OpenCV support through `OpenCvSharp4` and `OpenCvSharp4.runtime.win`.
- Proved direct camera capture from the Insta360/camera rail setup.
- Confirmed the Insta360 app must be closed before the capture tool can access the camera.
- Generated printable capture sheet prototypes.
- Current preferred sheet is a US Letter 3x2 horizontal-card layout in `D:\MY AI\Lobby\Outbox\card-imaging-sheet-letter-horizontal-3x2.pdf`.
- Created a first `process` command that outputs six files named `A1.jpg`, `A2.jpg`, `B1.jpg`, `B2.jpg`, `C1.jpg`, and `C2.jpg`.

Important result:

The pipeline can capture a camera image, detect the printed page well enough for first-pass perspective correction, and create six separate image files. This proves the acquisition pipeline plumbing.

Important failure:

The first crop implementation is geometrically poor. It can distort card width/height and is not suitable for final card scans. The current fixed-coordinate crop approach should be replaced by per-slot rectangle detection and per-card perspective correction.

Next technical direction:

- Preserve raw captures.
- Detect page fiducials or page corners.
- Correct page perspective.
- Detect each slot's actual black rectangle/corner anchors.
- Perspective-transform each card individually to a fixed card ratio such as `1400x1000` for horizontal scans.
- Add glare/blur/crop-confidence scoring before OCR.
- Add OCR and checklist matching only after geometry is stable.

Architecture note:

Local Windows capture should remain first because camera hardware access is simplest outside Docker. Later, folder-based image processing can move into MCP/Docker AI Hotel tooling.
