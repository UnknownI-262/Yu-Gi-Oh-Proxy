# YUGIOH Proxy

A single-file web app that turns a Yu-Gi-Oh! deck list into a print-ready A4 PDF with dotted cut lines. Search a card, add it to the sheet, and download. No install, no build step, no backend.

## Test it using the link belos
- [the lin](https://unknowni-262.github.io/Yu-Gi-Oh-Proxy/)

## Features

- **Card search** powered by the [YGOPRODeck API](https://ygoprodeck.com/api-guide/)
- **Alt art support**: every alternate artwork shows up as its own result, and you can switch a card's art right from the sheet
- **Auto grid layout** on A4 at the standard 59 × 86 mm card size (3 × 3, so 9 cards per page by default)
- **Live preview** of every page, including empty slots
- **Dotted cut lines** between cards to guide scissors
- **Adjustable margin and gap** between cards
- **Quantity controls** per card, with extra pages added automatically
- **PDF export** in the browser with [jsPDF](https://github.com/parallax/jsPDF); duplicate cards are embedded once to keep file size down
- **Dark and light mode**, following your system theme
- Works on mobile and desktop

## Usage

1. Open `index.html` in any modern browser (or use the GitHub Pages link, if enabled).
2. Type a card name in **Find cards** and tap a result to add it.
3. Adjust quantity and artwork in the **Sheet** list.
4. Tweak margin, gap, and cut lines under **Layout**.
5. Tap **Download PDF** and print at **100% scale** (turn off "fit to page").

Rename `proxy-sheet.html` to `index.html` if you want to host it on GitHub Pages.

## Print tips

- Print at 100% or "actual size". Any scaling changes the card dimensions and they won't fit sleeves.
- Use A4 paper. Letter-size support isn't included yet.
- Card images are about 421 × 614 px, roughly 180 dpi at 59 mm. That's good for playtesting, not for photo-quality prints.
- For sleeves, a heavier paper or a real card behind the proxy works best.

## How it works

| Part | What it does |
| --- | --- |
| `searchCards()` | Queries `cardinfo.php?fname=` and returns one entry per artwork |
| `computeLayout()` | Pure function that works out columns, rows, centered slot positions, and cut-line coordinates from the margin and gap. The preview and PDF both use it, so they always match |
| `paginate()` | Splits the expanded card list into pages |
| `exportPDF()` | Loads each unique image once, places cards with jsPDF, and draws the cut lines |

Everything lives in one HTML file, with a `CONFIG` object at the top for card size, page size, and proxy settings.

## Image loading and CORS

YGOPRODeck's image server doesn't send CORS headers, so browsers can't read the images directly for the PDF. On export the app tries a direct fetch first, then falls back to public image proxies ([weserv.nl](https://images.weserv.nl/) and [corsproxy.io](https://corsproxy.io/)). That means card images pass through those services during export.

If you want to avoid that, set your own proxy in `CONFIG.imageProxy`, or download and host the card images yourself. YGOPRODeck also asks that apps [store pulled data locally](https://ygoprodeck.com/api-guide/) instead of hotlinking, and rate-limits the API to 20 requests per second.

## Roadmap

- [ ] Paste or import a deck list (`.ydk`, text)
- [ ] Other card games (the search is isolated in `searchCards()`, so adding a source means swapping one function)
- [ ] Letter paper size and custom card sizes
- [ ] Bleed and crop marks
- [ ] Local image caching

## Disclaimer

This is an unofficial fan tool for **casual playtesting and proxy decks**. It is not affiliated with or endorsed by Konami or YGOPRODeck. Yu-Gi-Oh! is a trademark of Konami Digital Entertainment. Card images and data come from the YGOPRODeck API and belong to their respective owners. Please don't use proxies in sanctioned tournaments or to sell counterfeit cards.

