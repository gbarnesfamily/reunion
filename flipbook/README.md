# Family History Flipbook — Updated Files

This folder contains an updated version of the flipbook ready to deploy to
your `family-history` repo.

## What's new

- **Photo uploads** — Drop photos into `photos/` and add one line per person
  to show them on the page
- **Marriage dates** — Add dates to the `MARRIAGES` map to show them on
  spouse blocks
- **Fixed sidebar collapse** — Generation sections in the left nav now
  collapse and expand properly
- **"Descendants" labels** — Generations 2-6 are now labeled as Descendants
  (e.g., "Third Generation Descendant")
- **Cleaner HTML** — Proper `<body>` tag and better organized code
- **Data in a separate file** — The person data now lives in `data.js`,
  making the main `index.html` much easier to read and edit

## Files in this folder

| File | Purpose |
|------|---------|
| `index.html` | The main flipbook page — drop this in your repo |
| `photos/` | Folder for family photos |
| `README.md` | This file |

**Note:** You also need `data.js`. It's NOT provided here — you create it
from your existing flipbook (see below). Once created, it lives next to
`index.html` in your `family-history` repo.

## Step-by-step deployment

### 1. Create `data.js` from your existing flipbook

Your existing flipbook already has all the person data baked into the HTML.
We just need to extract it into a separate `data.js` file.

**Easiest way — browser console:**

1. Open your current flipbook at `gbarnesfamily.github.io/family-history/`
2. Enter the password to unlock it
3. Press **F12** (or right-click → Inspect) to open Developer Tools
4. Click the **Console** tab
5. Paste this and press Enter:

```js
copy('const PAGES = ' + JSON.stringify(PAGES) + ';\nconst TOC = ' + JSON.stringify(TOC) + ';\n');
```

This copies the data to your clipboard.

6. Open a text editor, paste, and save the file as `data.js`

**Alternative — manual extraction:**

1. Open your current `index.html` in a text editor
2. Find the lines starting with `const PAGES = [` and `const TOC = [`
3. Copy both lines (they're long)
4. Paste into a new file called `data.js`
5. Save

### 2. Upload to your `family-history` repo

In your `family-history` repo, you should now have:

```
family-history/
├── index.html      ← new file from this folder
├── data.js         ← created in step 1
└── photos/         ← folder for family photos
```

Replace the old `index.html` with the new one. Add `data.js`. Create the
`photos/` folder if it doesn't exist.

### 3. Test it

Visit your flipbook URL and:
- Enter the password
- Click the generation headers in the sidebar to verify collapse/expand works
- Navigate a few pages to verify everything loads

---

## How to add a photo for someone

1. Save the photo to the `photos/` folder in your `family-history` repo.
   Any common format works (jpg, png, webp).
2. Open `index.html` and find the `PHOTOS` map near the top of the
   `<script>` block. It looks like this:

```js
const PHOTOS = {
  // 0: 'photos/george-barnes.jpg',
};
```

3. Uncomment and add an entry. The key is the page index (0 = George Barnes,
   1 = Ellen Barnes Gerald, 2 = Lurenda Massey, and so on):

```js
const PHOTOS = {
  0: 'photos/george-barnes.jpg',
  4: 'photos/lonnie-barnes.jpg',
  21: 'photos/mary-sanders.jpg',
};
```

**Tip:** The page counter at the top of each person's page (like "5 / 37")
tells you the page number. Subtract 1 to get the page index for the PHOTOS map.

If you want to remove a photo, just delete or comment out its line.

---

## How to add marriage dates

Open `index.html` and find the `MARRIAGES` map:

```js
const MARRIAGES = {
  // "Adeline Bryant Barnes": "15 Jun 1888",
};
```

Uncomment and add entries keyed by the spouse name exactly as it appears
on the page:

```js
const MARRIAGES = {
  "Adeline Bryant Barnes": "15 Jun 1888",
  "Charlotte Edwards Barnes": "1918",
  "Walter A Massey Sr": "1915",
};
```

If two different people have a spouse with the same name and you need
separate dates, prefix the key with `pageIndex:`:

```js
"4:Charlotte Edwards Barnes": "1918",
```

---

## Changing the password

The flipbook password is `barnes2026`. To change it, find this line near
the bottom of the `<script>` block in `index.html`:

```js
const PASSWORD = 'barnes2026';
```

Change the string to whatever you want. Then update the matching entry on
the main reunion site if you want them to stay in sync.
