# Preventivi Gasbeton

An installable web app for quoting autoclaved aerated concrete (AAC) blocks on site, and generating the PDF to hand to the customer.

It started from a concrete problem: a building materials salesman was building quotes in a spreadsheet, on his phone, in front of the customer. It worked, but pinch-zooming into 4 mm cells is no way to work, and a spreadsheet produces nothing presentable to leave with the buyer. The app keeps the same calculation engine — verified row by row against the original spreadsheet — and only changes the shell around it.

## Features

- Thickness picker rendered as a row of blocks whose width is proportional to the real thickness
- Quantities in pallets, square metres or pieces, converted automatically
- Two product lines with independent discount, VAT and freight regimes
- Adhesive calculation: kilograms, bags and pallets derived from the quote quantities
- PDF export with logo, unit price breakdown, totals with VAT, and pallets kept outside the total
- Works offline; data stays on the device, with JSON export and import

## Calculation model

```
net           = list price × (1 − discount)
freight per m² = (truck cost ÷ pallets per truck) ÷ m² per pallet
€/m²          = net + freight
€/piece       = €/m² × m² per pallet ÷ pieces per pallet
```

Pallets are excluded from the square metre price and charged separately. The adhesive follows a parallel chain driven by square metres, with consumption per m² depending on thickness.

## Design decisions

**PWA rather than a native app.** SwiftUI would have required a Mac to build and, without the App Store, reinstallation every seven days. For a form with calculations and a PDF export, an installable web app delivers the same result with no expiry and no intermediary.

**A single file, no build step.** Nothing to compile, no dependencies to keep current.

**No backend.** Data lives in `localStorage`, so price lists and margins never cross the network. A network-first service worker with cache fallback keeps the app usable where there is no signal.

Stack: vanilla HTML, CSS and JavaScript. [jsPDF](https://github.com/parallax/jsPDF) from a CDN for the PDF.

## Setup

Any static host. With GitHub Pages: `Settings → Pages → Deploy from a branch → main / (root)`. Then from Safari: `Share → Add to Home Screen`.

After each change, bump the `CACHE` constant in `sw.js`, otherwise the service worker keeps serving the cached version.

## Configuration

The list prices included are the ones published by the manufacturer. **Discounts and freight costs are deliberately set to zero**: these are commercial terms that vary from dealer to dealer. They are entered once from the app under `Listini`, or loaded from a pre-filled JSON backup via `Azienda → Backup`.

To confirm with your supplier: bags per adhesive pallet, and price per bag.

## License

[Apache License 2.0](LICENSE).

---

Unofficial tool, independently developed and not affiliated with any manufacturer. "Gasbeton" is a registered trademark of its respective owners, used here descriptively. The prices included are illustrative: always check the official price lists in force.
