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
