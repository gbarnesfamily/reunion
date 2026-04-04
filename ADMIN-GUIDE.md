# George Barnes Family Reunion 2026 — Admin Guide

This guide covers how the reunion website works, where everything lives, and how to make common changes.

---

## Website Overview

**Live site:** https://gbarnesfamily.github.io/reunion/  
**Repository:** https://github.com/gbarnesfamily/reunion  
**Deploys from:** `main` branch (GitHub Pages — any push to `main` triggers a rebuild)

The site is a single `index.html` file with all CSS and JavaScript inline. Images are stored in the `images/` folder.

---

## Key Pages / Sections

| Section | ID | Description |
|---------|-----|-------------|
| Home | `#home` | Hero with event name, dates, times, and RSVP button |
| Countdown | `#countdown` | Live countdown timer to July 10, 2026 |
| Details | `#details` | Event times, location, and nearby hotel info |
| Our Legacy | `#legacy` | Family reunion history (15 years since 2012) |
| RSVP | `#rsvp` | Registration form with attendance, guests, committees, dietary |
| Gallery | `#gallery` | Photo gallery with submission form |
| Contact | `#contact` | Committee email contact |

---

## Page Visibility (Admin Panel)

You can show/hide any section without changing code:

1. Go to `https://gbarnesfamily.github.io/reunion/?admin=true`
2. Click the **gear icon** (bottom right)
3. Enter password: `Reunion2026`
4. Toggle sections on/off
5. Click **Save & Publish Changes**

Visibility is stored in the browser's localStorage. To set defaults for first-time visitors, edit the `DEFAULT_VISIBILITY` object in the `<script>` section of `index.html`.

---

## Google Services

### RSVP Form

| Item | Location |
|------|----------|
| **Form action URL** | In `index.html`, search for `id="rsvp-form"` — the `action` attribute |
| **Apps Script** | Google Apps Script project (RSVP) |
| **Google Sheet** | Stores all RSVP responses |
| **Confirmation email** | Sent automatically via `sendConfirmationEmail()` in the Apps Script |

**Sheet columns:**
`Timestamp | First Name | Last Name | Email | Phone | SMS Opt-In | City | State | Attendance | Attending Friday | Attending Saturday | Guest Count | Total Registrants | Guests | Committees | Dietary | Message`

**When attendance is "No":** The form hides guests, committees, dietary, and attending days fields. The success message changes based on the response.

---

### Photo Gallery

| Item | Location |
|------|----------|
| **Gallery display + Photo upload** | Single Apps Script deployment (handles both `doGet` and `doPost`) |
| **Gallery Google Sheet** | https://docs.google.com/spreadsheets/d/12ZQBcbaVm6c6y-S3GiecP-xo7EYctIBPkFEyI10JH58/edit |
| **Drive folder** | "Reunion Photo Uploads" folder in Google Drive |
| **Site variables** | `GALLERY_SCRIPT_URL` and `PHOTO_UPLOAD_URL` in the `<script>` section of `index.html` |

**Sheet columns:**
`Timestamp | Submitted By | Email | Caption | Drive Link | Photo URL | Approved | Status`

#### How photos flow:
1. Family member submits photo(s) on the site (no Google account needed)
2. Photo is saved to the Google Drive folder
3. A row is added to the Gallery Sheet
4. Admin gets an email notification ("New Photo Submission - Barnes Reunion Gallery")
5. Admin reviews the photo and types **`yes`** in the `Approved` column
6. Photo appears on the site on next page load

#### Approving a photo:
- Type `yes` in the **Approved** column (any capitalization works)
- The `Photo URL` column is auto-populated when the photo is uploaded

#### Removing a photo from the gallery:
- Type `deleted` in the **Status** column
- The photo stays in Drive and the Sheet for records but won't display on the site
- To restore it, clear the Status cell

---

## How to Make Common Changes

### Update event times
Edit `index.html`:
- **Home page:** Search for `hero-date-label` — times are next to the dates
- **Details page:** Search for `details-item-time`

### Update location info
Search for `The Home of Mike and Janice Hicks` in `index.html`

### Update hotel listings
Search for `Nearby Lodging` in `index.html`. Each hotel is a `<li>` with name, address, Google Maps link, and phone number.

### Update the RSVP confirmation email
Edit the `sendConfirmationEmail()` function in the RSVP Apps Script. After changes:
1. Run `testEmail` to verify
2. Do **Deploy → New deployment**
3. Update the form action URL in `index.html` if the URL changes

### Change committee options
Search for `name="committee"` in `index.html`. Each checkbox is a `<label>` with an `<input>`.

### Update the contact email
Search for `mailto:g.barnes.family` in `index.html`

---

## Redeploying Apps Scripts

**Important:** Every time you do **Deploy → New deployment** in Apps Script, the URL changes. You must update the corresponding URL in `index.html`:

| Script | Variable in index.html |
|--------|----------------------|
| RSVP | `action` attribute on the `<form id="rsvp-form">` tag |
| Gallery (doGet) | `GALLERY_SCRIPT_URL` in the `<script>` section |
| Gallery (doPost) | `PHOTO_UPLOAD_URL` in the `<script>` section |

The Gallery doGet and doPost share a single deployment URL (same Apps Script project).

**Tip:** If you want to avoid URL changes, you can edit an existing deployment instead of creating a new one: **Deploy → Manage deployments → pencil icon → select new version → Deploy**

---

## File Structure

```
reunion/
  index.html          ← The entire website (HTML, CSS, JS)
  images/
    hero-bg.png        ← Hero section background
    reunion-logo.png   ← Coming soon overlay logo
    reunion-2016.jpg   ← Legacy section photo
    barnes-family-1.jpg
    barnes-family-2.jpg
  ADMIN-GUIDE.md       ← This file
  README.md
```

---

## Contact

**Committee email:** g.barnes.family@gmail.com  
**Website repo:** https://github.com/gbarnesfamily/reunion
