# Pozzo Manager

One installable app with three pages. Swipe left and right, or tap the tabs.

- **Sales** (`orders.html`): paste an order message, check it, save it. Builds the cash memo, the parcel entry file and takes products off stock.
- **Stock** (`stock.html`): products with codes like DUPH-001, purchases, issues, stock list import.
- **Stickers** (`stickers.html`): reads Steadfast label PDFs and builds the parcel tag Excel.

Files in this repo: `index.html`, `orders.html`, `stock.html`, `stickers.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`, `README.md`.

**Never upload `pozzo-script.txt` here.** It belongs only in Google Apps Script, because it holds your secret key.

---

## Setup, step by step

### A. Google (sign in to the Pozzo Toys Google account in Chrome)

1. Open script.google.com and tap **New project**. Name it "Pozzo Manager".
2. Delete the sample code. Paste the whole of `pozzo-script.txt`.
3. On line 1, replace `CHANGE-THIS-SECRET` with your secret key. Keep the quote marks. Save.
4. Tap the gear icon (**Project Settings**). Turn on **Show "appsscript.json" manifest file in editor**. Go back to the editor, open `appsscript.json`, and set `"timeZone": "Asia/Dhaka"`. Save.
5. In the function menu choose **setup** and tap **Run**. Google asks for permission: tap **Review permissions**, choose the account, then **Advanced**, then **Go to Pozzo Manager (unsafe)**, then **Allow**. The "unsafe" warning appears for every personal script.
6. Check Google Drive. You should see a folder **Pozzo Toys Data** with this month's sales file and **Pozzo Stock**.
7. Choose **installDailyEmail** and tap **Run**. This sends the stock check email at 9 PM Dhaka time to this account.
8. Tap **Deploy**, then **New deployment**, the gear icon, **Web app**. Set **Execute as: Me** and **Who has access: Anyone**. Tap **Deploy** and copy the Web app link (it ends with `/exec`).

If you ever delete the Stock or a Sales file in Drive, run **clearCache** and then **setup**, so the script forgets the old file.

To change the script later: **Deploy**, **Manage deployments**, the pencil, **Version: New version**, **Deploy**. The link stays the same.

### B. GitHub

1. Make the new GitHub account. Tap **New repository**, name it (for example `pozzo-manager`), set it to **Public**.
2. **Add file**, **Upload files**. Upload the nine files listed at the top. Commit to `main`.
3. **Settings**, **Pages**, Source: **Deploy from a branch**, branch `main`, folder `/ (root)`, **Save**.
4. After one or two minutes your app is at `https://<your-username>.github.io/<repo-name>/`.

To update later: upload the changed files and commit. Open the app twice to load the new version.

### C. Your phone

1. Open the link in Chrome. Menu, **Install app** (or **Add to Home screen**).
2. Sales tab, **Connection settings**. Paste the Web app link and your secret key. Save.

---

## Updating from v2 to v3

1. Upload all the changed files to GitHub and commit. Open the app twice.
2. In the script editor, replace the whole script with the new `pozzo-script.txt`, set your key on line 1, and keep `"timeZone": "Asia/Dhaka"` in `appsscript.json`.
3. Run **setup** once. It adds the new column headings to the Purchases tab. Your data is not changed.
4. **Deploy**, **Manage deployments**, the pencil, **Version: New version**, **Deploy**.

## What is new in v3

- **Faster Stock page.** One request loads everything. Switching between Stock, To check and Issues no longer asks Google again. Use **↻ Refresh** to force a fresh load. The script remembers where the files are and keeps the list ready for 5 minutes, so right after you change something on another device, tap Refresh.
- **No more jams.** Reading requests no longer wait for the lock. The app sends one request at a time, shows "Working…", and after 45 seconds says "Google is slow to answer" with a **Try again** button.
- **Sales.** The memo appears as soon as the order is saved. Stock updates in the background ("Updating stock…"). The parcel file asks "Did the parcel file download?" before marking orders as exported.
- **Voiding a purchase** now puts back the average cost, and the old selling price when no later change touched it. The message says what was restored.
- **New memo design** (sky blue, white page, your logo).
- **Slow requests:** open **Executions** in the script editor, open a row, and the log shows the action name and how long it took.

## First-run tests (do these before real orders)

1. Save 2 or 3 fake orders. Check the sheet rows, IDs like `PZ26OCT-001`, and the totals.
2. Save one order with a part advance and one fully prepaid. Check COD on screen and the parcel file Amount.
3. Open the memo. Check the Bangla names on your phone, then PNG, PDF and Share.
4. Upload the parcel file to Steadfast, including one with Amount 0.
5. Run one real label PDF in Stickers. Check the Bangla OCR.
6. Import a small stock list. Then make a purchase with a name variant (for example "Rotating Jenga" for "Rotating Tower Jenga") and check the confirm page.
7. Swipe between the pages on your phone.

## Rules to remember

- **Do not hand-edit the stock sheet.** Stock is the sum of the Ledger tab. Use **Import stock list** for the first load and **Adjust stock** for corrections. Do not edit the Names tab either; use Edit product.
- **Order IDs** look like `PZ26OCT-014`. Purchase IDs look like `PU26OCT-001`. Serials restart at 001 each month.
- **Product codes** are 4 letters and a serial (DUPH-001). The code never changes, even if you rename the product.
- **Memo shop details** (page name and phone) are the `SHOP` line near the top of the memo code in `orders.html`.
- Saving orders needs internet. Nothing is saved if the sheet cannot be reached.
- Turn on 2-step verification and set a recovery phone and email on the Pozzo Toys Google account. All records live there.

## Privacy

Label PDFs are read on the phone and never uploaded. Orders and stock go only to your own Google Sheets. The cash memo is drawn on the phone and is not stored.
