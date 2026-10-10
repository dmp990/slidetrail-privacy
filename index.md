# SlideTrail privacy

SlideTrail samples the YouTube tab you explicitly activate through the extension. It crops frames to the video or your selected slide region, stores snapshots and notes locally, and displays them in Chrome's side panel.

The full tab stream is processed temporarily in an offscreen extension document. Only cropped slide snapshots are saved. Automatic capture skips ads and hidden source tabs. Chrome displays its own capture indicator.

## Data stored on your device

SlideTrail saves lecture titles and URLs, slide images and thumbnails, image comparison signatures, timestamps and repeated appearances, pins, notes, and capture preferences. These are used only to provide slide capture, history, annotations, and your requested exports.

Persistent lecture, slide, and preference records are encrypted with AES-256-GCM before they enter IndexedDB. Each write uses a new random 96-bit nonce and a 128-bit authentication tag. Record identifiers use keyed HMAC-SHA-256 values so the database indexes do not expose lecture URLs or video IDs.

Two non-extractable Web Crypto keys are stored in the extension's local IndexedDB vault. They allow automatic unlocking without a password and are not sent to a server or synced to another device. Non-extractable means JavaScript cannot export the keys through Web Crypto; the keys remain part of the browser profile. This design does not protect data from someone who controls that profile or can execute code within the extension. Losing the keys makes the encrypted records unreadable.

Active capture state is held in Chrome session storage while Chrome is running. Frames and decrypted records are available in memory while the extension uses or displays them.

## Migration and retention

Existing plaintext lecture and slide records are replaced automatically on first storage access. The original records are cleared only in the same transaction that commits their encrypted replacements. Old Chrome local-storage capture preferences are removed after encrypted copies have been saved successfully.

Migration and deletion remove logical records. They cannot guarantee secure erasure of earlier storage-engine pages, device backups, or files you previously exported.

You can remove saved slides individually, clear a lecture, or remove the extension. Removing a slide deletes its saved image, thumbnail, note, pin, signatures, and timestamps. A temporary Undo option holds the removed record in memory and can restore it. Clear lecture deletes its stored slides, notes, lecture information, and remembered crop. Removing the extension removes its browser storage and local keys.

## Transfers and exports

No captured images, notes, lecture URLs, keys, or browsing data are sent to a backend. There is no analytics or AI service. YouTube's own network traffic is independent of SlideTrail.

Downloading a slide or exporting notes creates ordinary, unencrypted JPEG or Markdown files on your device at your request. Exported files remain after clearing a lecture or uninstalling the extension.

SlideTrail uses data only for its stated features. It does not sell user data, use it for advertising, or transfer it for unrelated purposes. Its use of user data complies with the Chrome Web Store User Data Policy, including the Limited Use requirements.

Capture permission is temporary and tied to toolbar activation. Capture does not restart automatically after browser restart, navigation, or reload.
