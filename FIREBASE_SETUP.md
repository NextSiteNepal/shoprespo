# Firebase Setup Guide — Sandesh Sun Chandi Pasal

## 1. Firebase Project — Already Created ✅
Your project: `sandeshsunpasal`

---

## 2. Admin Login — No Firebase Auth Needed

The admin login is now a simple hardcoded check inside `admin/index.html` (no Firebase Authentication setup required at all).

- Email: `sandeshsunpasal@gmail.com`
- Password: `sandeshshah@13579`

To change either value later, open `admin/index.html`, find these two lines near the top of the `<script type="module">` block, and edit them directly:
```js
const ADMIN_EMAIL = "sandeshsunpasal@gmail.com";
const ADMIN_PASSWORD = "sandeshshah@13579";
```

⚠️ **Security note:** Because there's no real backend authentication, anyone who views the page source can see these credentials, and the Firestore rules below allow anyone to write data (not just logged-in admins) — this trade-off was chosen for simplicity/speed. Don't share the admin URL publicly, and keep in mind a technically determined visitor could edit your products/images even without the password. If you want real protection later, Firebase Authentication (email/password, free on Spark) can be added back in.

---

## 3. Create Firestore Database

1. Click **Firestore Database** in the sidebar
2. Click **Create database**
3. Choose **Start in production mode**
4. Select region: **asia-south1 (Mumbai)** (closest to Nepal) → Enable

### Paste these Security Rules:
Go to **Firestore Database → Rules** tab and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Open read/write — no Firebase Auth is used (see admin login note above)
    match /products/{productId} {
      allow read, write: if true;
    }

    match /collections/{collectionId} {
      allow read, write: if true;
    }

    match /categories/{categoryId} {
      allow read, write: if true;
    }

    match /siteSettings/{settingId} {
      allow read, write: if true;
    }
  }
}
```

Click **Publish**

---

## 4. Image Hosting (No Firebase Storage Needed)

Firebase Storage now requires a billing account to be enabled, even for free-tier usage, so this site does NOT use it. Product/collection images are stored as plain URLs in Firestore — host the actual image files anywhere free:

- **imgbb.com** (recommended) — no signup required, drag & drop an image, copy the **direct image link**
- Or any other free image host that gives you a direct `.jpg`/`.png` link

Just paste that link into the Admin Panel's image URL field. No Firebase Storage, no billing account, no Blaze plan required.

---

## 5. Initial Data — Seed Collections & Categories

In **Firestore → collections** collection, create these documents manually:

**Document ID: gold**
```json
{
  "name": "Gold Collection",
  "subtitle": "Pure Gold",
  "description": "Certified 24K gold jewelry"
}
```

**Document ID: silver**
```json
{
  "name": "Silver Collection",
  "subtitle": "Sterling Silver",
  "description": "Handcrafted 925 sterling silver"
}
```

**Document ID: wedding**
```json
{
  "name": "Wedding Collection",
  "subtitle": "Bridal Special",
  "description": "Complete bridal sets"
}
```

In **Firestore → categories** collection:

| Document ID | name |
|---|---|
| rings | Rings |
| necklaces | Necklaces |
| earrings | Earrings |
| bracelets | Bracelets |
| bangles | Bangles |
| chains | Chains |
| pendants | Pendants |
| nose-pins | Nose Pins |
| anklets | Anklets |
| coins | Coins |
| others | Others |

---

## 6. Uploading Product Images

1. Go to **Storage** in Firebase Console
2. Create a folder called `products/`
3. Upload images (JPG/PNG, under 5MB recommended)
4. After upload, click the image → **Copy download URL**
5. Paste those URLs into the Admin Panel when adding products

---

## 7. Deploy to Vercel

### Option A: Drag & Drop (Easiest)
1. Go to [vercel.com](https://vercel.com) and sign up/log in
2. Click **Add New → Project**
3. Click **"Import from folder"** or drag the entire project folder
4. Click **Deploy** — done!

### Option B: Via GitHub
1. Push the project to a GitHub repo
2. Connect to Vercel → import that repo
3. Deploy

Your site will be live at a URL like `sandesh.vercel.app`

---

## 8. File Structure

```
/
├── index.html              ← Main homepage (Nepali title now BLACK)
├── favicon.svg
├── images/
│   ├── shop-interior.png
│   ├── shop-storefront.png
│   └── shop-storefront-2.png
├── collections/
│   ├── gold.html           ← /collections/gold
│   ├── silver.html         ← /collections/silver
│   └── wedding.html        ← /collections/wedding
└── admin/
    └── index.html          ← /admin (login-protected)
```

---

## 9. Admin Usage

1. Open `yoursite.com/admin`
2. Log in with the admin email/password you created in Firebase Auth
3. **Products tab**: Add/Edit/Delete products. Fill in Title, Order Number, Collection, Category, and paste image URLs from Firebase Storage
4. **Collections tab**: Manage the Gold/Silver/Wedding collections
5. **Categories tab**: Add new categories — they appear live on collection pages instantly

### WhatsApp Order Button
When visitors click "Order Now", they are taken directly to WhatsApp with:
```
Hello, I would like to order:

Product Name: [product title]
Collection: [collection]
Category: [category]
Order Number: [order number]
```
Messages go to: **+977 9843022602**
