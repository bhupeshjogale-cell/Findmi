# FindMi — campus lost-and-found tag system

A finder scans a QR code or taps an NFC sticker on your bottle, ID card,
or laptop. It opens a page telling them whose item it is, with one button
that opens WhatsApp with a message already written. No app, no backend,
no signup.

Files in this folder:
- `index.html` — the page a finder sees when they scan a tag
- `tags.json` — the data: which tag ID belongs to which owner/item/phone
- `generate_qr.py` — optional script to batch-print QR codes from `tags.json`

## 1. Edit your tag data

Open `tags.json` and replace the sample entries with real ones. Each key
is a tag ID you make up (keep it short, e.g. `AB123`):

```json
"AB123": {
  "owner": "Your Name",
  "item": "Water Bottle",
  "phone": "91XXXXXXXXXX"
}
```

`phone` must be in international format with no `+`, spaces, or leading
zero — e.g. a Mumbai number `98765 43210` becomes `919876543210`.

Add as many tag IDs as you need — one per physical item.

## 2. Host it for free on GitHub Pages

1. Create a new GitHub repo (e.g. `findmi`).
2. Upload `index.html` and `tags.json` to it.
3. Go to **Settings → Pages**, set source to the `main` branch, root folder.
4. GitHub gives you a URL like:
   `https://yourusername.github.io/findmi/`

Each tag's page is that URL plus `?id=` and the tag ID, e.g.:
`https://yourusername.github.io/findmi/?id=AB123`

## 3. Generate QR codes

Easiest: paste each tag's full URL (from step 2) into any free QR
generator site and download the image to print on sticker paper.

Or, if you want to batch-generate all of them at once, use the included
script:

```bash
pip install qrcode[pil]
python3 generate_qr.py
```

This reads every tag ID in `tags.json`, asks for your GitHub Pages base
URL once, and drops one PNG per tag into a `qr_codes/` folder — ready to
print.

## 4. Write the NFC tags (optional, for the "tap instead of scan" demo)

1. Buy blank NTAG213 stickers (~₹10–15 each).
2. Install the free **NFC Tools** app (Android, Play Store).
3. Open NFC Tools → Write → Add a record → URL/URI → paste the same tag
   URL you used for the QR code (e.g. `.../?id=AB123`).
4. Tap **Write** and hold the sticker to your phone's back until it confirms.

## 5. Stick tags on your demo items

Put a QR sticker (and NFC sticker, if made) on each item you're bringing
to the pitch. Test every tag by scanning it yourself before the demo —
confirm the WhatsApp button opens with the right name and item.

## Notes

- This is fully static — there's no server, so anyone who has the link
  can see the JSON. Fine for a prototype demo; for a real product you'd
  move `tags.json` into a proper database with access control.
- The WhatsApp button opens `wa.me`, which works without either side
  having saved the other's number or installed anything beyond WhatsApp
  itself.
