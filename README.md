# श्री शिवनेरी ढाबा — Shree Shivneri Dhaba (Parcel Seva) · QR Menu

Digital QR menu for **Shree Shivneri Dhaba**, built from the Hotel Ruchkar Katta QR-menu template.
Plain static site (HTML, CSS, vanilla JS) for **GitHub Pages**: no build step, no server, no external requests.

This is a **separate project**. It shares no files, links or data with the Ruchkar Katta repository or website.

- Restaurant details: `data/restaurant.json`
- Menu items and prices: `data/menu.json` (31 items, from the printed menu)
- Logo: `assets/logo/shree-shivneri-logo.png` (the supplied logo; only the empty white border was trimmed)
- How to edit the menu yourself: `docs/CUSTOMER_SETUP.md`

---

## What changed from the Ruchkar Katta template

All changes are data-driven. Nothing about this client is hard-coded in the JavaScript.

| Change | Where | Configured by |
| --- | --- | --- |
| Thin scrolling announcement strip at the top of the header (pauses on hover/tap, static when the phone has "reduce motion" on) | `index.html`, `js/ui.js` → `renderTicker`, `css/style.css` | `announcements` in `restaurant.json` |
| Second restaurant name under the Marathi name | `index.html`, `js/ui.js` | `altName` |
| Price shown exactly as printed, e.g. `₹450/270` (the template's numeric `price` would have turned `450/270` into `450270`) | `js/menu.js`, `js/ui.js` → `priceBlock` | `priceText` on an item |
| English name under each Marathi dish name; English or Marathi search both work | `js/menu.js`, `js/ui.js` | `nameEn` on an item |
| Correct Marathi font and language tags | `css/style.css`, `js/ui.js` | `contentLang: "mr"` |
| Logo in a rounded white tile instead of a circle (suits the octagon logo) | `css/style.css`, `js/app.js` | `theme.logoShape: "rounded"` |
| Filter chips that would not change the list are hidden automatically. On this pure-veg menu every chip (Veg, Non-Veg, Available, Bestsellers, New) is hidden, so the filter row does not show | `js/app.js` → `pruneFilters` | automatic |
| Compact two-column cards when a section has no photos | `css/style.css` | automatic |
| Colours: primary `#8B0000`, secondary `#FFF0C9`, gold accent `#C9A227` | — | `theme` |

Not copied from Ruchkar Katta: its `data/` files, food photos, logo, QR code image, phone, address, email, social links, hours and site URL.

**Dish photos:** every dish already points to `assets/menu/<dish-id>.jpg` (full list in `assets/menu/PHOTO-FILENAMES.txt`). Until a photo is uploaded with that exact name, the card shows a placeholder with the dish's first letter. Upload photos to `assets/menu/` on GitHub with those names and they appear automatically; no code or JSON edits needed. Use landscape 4:3, about 800×600, under 150 KB each. If a photo is a stock image rather than the dhaba's own dish, label it as representational.

---

## Items to confirm with the client

Until confirmed, these are shown exactly as printed.

1. **पनीर क्रेश — ₹200**: second word is unclear in the photo. Confirm the spelling (English shown as "Paneer Kresh").
2. **काजू करी (sweet) — ₹160**: "(sweet)" is mostly hidden by glare. Confirm.
3. **Two-price items** (`450/270`: स्पे पनीर मालवणी, पनीर हंडी, स्पे व्हेज मालवणी, व्हेज हंडी; `100/50`: प्लेन राईस, जिरा राईस). Ask what the two prices mean (for example full/half). If confirmed, switch them to `variations` so each price gets a label.
4. **"स्पे"** on 3 items: keep the abbreviation or write "स्पेशल"?
5. **"वेळ- 11:11"** on the menu: opening time, hours, or branding? Opening hours are empty, so no Open/Closed label shows yet.
6. **Spellings**: the menu uses both मसूरा/मसुरा and दाल/डाल. Kept as printed.
7. **Location link** `https://share.google/AtnjZuBJCP1YNddHe` is used for the Location button. Open it on a phone and confirm it is the dhaba's Google listing.
8. **Not yet provided**: address, opening hours, email, Swiggy/Zomato links (both logos are on the printed menu), published site URL.
9. **Availability**: every dish is marked available (template default), so the dish popup says "Available today". Set `"isAvailable": false` on any dish that is not served.

Category grouping (मसूर स्पेशल, पनीर, व्हेज भाज्या, डाल, भात, रोटी) was chosen by me; the printed menu has no categories. Rename or regroup freely in `data/menu.json`.

---

## Test locally

The menu loads JSON with `fetch`, so it needs a small local server (double-clicking `index.html` shows an error).

```bash
cd Shree-Shivneri-Dhaba
python -m http.server 8000
```

Open http://localhost:8000.

To test on your phone before publishing: connect the phone to the same Wi-Fi, find your computer's IP (`ipconfig` on Windows), and open `http://<your-ip>:8000` on the phone.

---

## Deploy to GitHub Pages (new repository)

Do this in a **new** repository. Do not push into the Ruchkar Katta repository.

1. On github.com click **New repository**.
   - Name: `Shree-Shivneri-Dhaba` (any name works; it becomes part of the URL)
   - Visibility: **Public**
   - Do not add a README (this project has one)
2. Upload the files. Either:
   - **Browser:** open the new repo → **Add file → Upload files** → drag in the *contents* of this folder (including `.nojekyll`) → **Commit changes**. On Windows, hidden `.nojekyll` may not drag in; if so, create it on GitHub with **Add file → Create new file**, name `.nojekyll`, leave it empty.
   - **Git:**
     ```bash
     cd Shree-Shivneri-Dhaba
     git init
     git add .
     git commit -m "Shree Shivneri Dhaba QR menu"
     git branch -M main
     git remote add origin https://github.com/<your-username>/Shree-Shivneri-Dhaba.git
     git push -u origin main
     ```
3. In the repo go to **Settings → Pages**. Under **Build and deployment**, choose **Source: Deploy from a branch**, branch **main**, folder **/ (root)**, and save.
4. Wait 1–2 minutes. The address appears at the top of the Pages settings, normally
   `https://<your-username>.github.io/Shree-Shivneri-Dhaba/`.
5. Open that address on a phone and check it (checklist below).
6. Put the confirmed address into `siteUrl` in `data/restaurant.json` and commit. This sets the link-preview and Google URL.

All paths in the project are relative, so it works at that sub-folder address without any base-path settings.

---

## Create the QR code (only after the site is live)

1. Copy the **final live address** from step 4 above, and confirm it opens on mobile data (not just Wi-Fi).
2. Make the QR from that exact address (for example with a free generator such as qr-code-generator.com or the QR option in Chrome's share menu). Use a **static** QR that encodes the URL directly, not a "dynamic" QR that redirects through a third party.
3. Download a high-resolution PNG or SVG for printing.
4. Scan the printed proof with at least two phones before printing in bulk.

The QR never needs to change: menu and price edits update the same address.

---

## Mobile and QR testing checklist

**QR**
- [ ] QR scans with Android camera and iPhone camera
- [ ] It opens the Shree Shivneri address, not Ruchkar Katta
- [ ] Works on mobile data with Wi-Fi off
- [ ] Printed QR scans from about 30 cm, in normal restaurant light

**Page**
- [ ] Logo, "श्री शिवनेरी ढाबा", "SHREE SHIVNERI DHABA" and "Parcel Seva" show correctly
- [ ] Marathi text renders cleanly (no boxes or broken letters)
- [ ] Announcement strip scrolls smoothly and shows all three messages
- [ ] All 6 category tabs work and highlight while scrolling
- [ ] All 31 dishes appear, with prices matching the printed menu
- [ ] `450/270` and `100/50` prices show exactly as printed
- [ ] Every dish photo matches its dish name (no placeholder letters left)
- [ ] Search works for "paneer", "पनीर", "dal", "malvani"
- [ ] Tapping a dish opens its details; close button and back gesture close it
- [ ] No sideways scrolling on a small phone

**Buttons**
- [ ] **Call** opens the dialler with 7079611555
- [ ] **WhatsApp** opens a chat with 7079611555 with the greeting pre-filled
- [ ] **Location** opens the dhaba's Google listing
- [ ] **Instagram** (footer) opens @shri_shivneri_dhaba_parcel_

**Devices**
- [ ] Android Chrome, iPhone Safari, one tablet, one desktop browser
- [ ] Sharing the link on WhatsApp shows the restaurant name in the preview (after `siteUrl` is set)
