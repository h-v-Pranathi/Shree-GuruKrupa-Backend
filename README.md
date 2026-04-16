# Shree GuruKrupa — Backend Setup Guide

## What This Backend Does
- Hosts your website (index.html, gallery.html) as a real server
- Gives you a private Admin Panel to upload/delete gallery stones
- Stores stone images on Cloudinary (free cloud storage)
- Stores stone data in MongoDB Atlas (free cloud database)
- Homepage and gallery page load items live from the database

---

## Step 1 — Install Node.js

Download and install Node.js from: https://nodejs.org
Choose the **LTS** version.

Verify installation:
```
node --version
npm --version
```

---

## Step 2 — Get a Free MongoDB Atlas Database

1. Go to https://cloud.mongodb.com
2. Create a free account
3. Create a free **M0** cluster (any region)
4. Click **Connect** → **Drivers** → copy the connection string
5. It looks like:
   ```
   mongodb+srv://youruser:yourpassword@cluster0.xxxxx.mongodb.net/gurukrupa
   ```
6. Make sure to replace `<password>` with your real password in the string

---

## Step 3 — Get Free Cloudinary Account

1. Go to https://cloudinary.com and create a free account
2. From your Dashboard, copy:
   - **Cloud Name**
   - **API Key**
   - **API Secret**

---

## Step 4 — Fill In Your .env File

Open the `.env` file in the project folder and fill in:

```
MONGODB_URI=mongodb+srv://youruser:yourpassword@cluster0.xxxxx.mongodb.net/gurukrupa
JWT_SECRET=any_long_random_string_you_choose
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret
ADMIN_USERNAME=admin
ADMIN_PASSWORD=YourStrongPassword@123
```

**Important:** Never share your .env file with anyone.

---

## Step 5 — Install Dependencies

Open a terminal/command prompt inside the project folder and run:

```bash
npm install
```

This downloads all required packages (Express, MongoDB, Cloudinary, etc.)

---

## Step 6 — Create Admin User (Run Once)

```bash
node server/seed.js
```

This creates your admin login credentials in the database.
You only need to run this **once**.

---

## Step 7 — Start the Server

```bash
npm start
```

You will see:
```
✅  MongoDB connected
🚀  Server running at http://localhost:3000
🔐  Admin panel at  http://localhost:3000/admin/login.html
```

---

## Step 8 — Open Your Website

| Page | URL |
|---|---|
| Main website | http://localhost:3000 |
| Full gallery | http://localhost:3000/gallery.html |
| Admin login | http://localhost:3000/admin/login.html |
| Admin dashboard | http://localhost:3000/admin/dashboard.html |

---

## How to Use the Admin Panel

1. Go to http://localhost:3000/admin/login.html
2. Enter your username and password (from .env)
3. You are now in the dashboard
4. To **add a stone**:
   - Fill in the name and category
   - Optionally write a description
   - Tick "Show on Homepage" if you want it in the featured 6
   - Upload an image (JPG/PNG/WEBP)
   - Click Upload
5. The stone appears live on gallery.html immediately
6. To **delete a stone**: click the Delete button on any card

---

## How the Gallery Works (Smart Fallback)

- **Server running** → gallery.html and index.html load items from the database automatically
- **Server not running** → the existing static HTML cards are shown (the website still works!)

This means your website works with OR without the server — no risk of a blank page.

---

## Running on a Real Server (Deployment)

To put this online so anyone can visit it:

**Option A — Railway (Easiest, Free tier available)**
1. Go to https://railway.app
2. Connect your GitHub repo
3. Add your .env variables in Railway's dashboard
4. Deploy — Railway gives you a public URL

**Option B — Render**
1. Go to https://render.com
2. New Web Service → connect GitHub
3. Build command: `npm install`
4. Start command: `npm start`
5. Add environment variables from your .env

**Option C — VPS (DigitalOcean / Hostinger)**
1. Upload files via FTP or Git
2. Run `npm install` and `npm start`
3. Use PM2 to keep it running: `npm install -g pm2 && pm2 start server/server.js`

---

## File Structure

```
gurukrupa-backend/
├── index.html              ← Main website (homepage)
├── gallery.html            ← Full gallery page
├── style.css               ← All styles
├── guru/                   ← Your local images
├── admin/
│   ├── login.html          ← Admin login (private)
│   └── dashboard.html      ← Admin panel (private)
├── server/
│   ├── server.js           ← Express server
│   ├── seed.js             ← Run once to create admin user
│   ├── config/
│   │   └── cloudinary.js   ← Image upload config
│   ├── models/
│   │   ├── Stone.js        ← Gallery item schema
│   │   └── User.js         ← Admin user schema
│   ├── middleware/
│   │   └── authMiddleware.js
│   └── routes/
│       ├── auth.js         ← Login/logout API
│       └── gallery.js      ← Gallery CRUD API
├── package.json
├── .env                    ← Your secret credentials (never share)
└── .gitignore
```

---

## API Endpoints (for reference)

| Method | URL | Access | Description |
|---|---|---|---|
| POST | /api/auth/login | Public | Admin login |
| GET | /api/auth/verify | Private | Check token |
| GET | /api/gallery | Public | Get all stones |
| GET | /api/gallery/featured | Public | Get featured 6 |
| POST | /api/gallery | Admin | Add new stone |
| PUT | /api/gallery/:id | Admin | Edit stone |
| DELETE | /api/gallery/:id | Admin | Delete stone |

---

## Support

If you have any issues:
- Make sure MongoDB URI is correct and your IP is whitelisted in Atlas
- Make sure Cloudinary credentials are correct
- Check the terminal for error messages — they are descriptive
