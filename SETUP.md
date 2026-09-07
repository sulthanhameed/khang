# 🛠 Complete Setup Guide

**Do these steps in ORDER** to get everything working locally.

---

## ⚠️ Prerequisites

You need these installed on your computer:

### 1. Node.js (Required)
Download from https://nodejs.org — install the **LTS version (20.x)**.

Check installation:
```bash
node --version
# Should show v20.x.x or higher

npm --version
# Should show 10.x.x or higher
```

### 2. Git (Required)
Download from https://git-scm.com — install with default options.

Check installation:
```bash
git --version
```

### 3. VS Code (Recommended)
Download from https://code.visualstudio.com

---

## 📥 Step 1: Get the Code

Open VS Code terminal (`` Ctrl+` ``) in `C:\Projects\` (NOT OneDrive!):

```bash
git clone https://github.com/sulthanhameed/hameed.git raaha
cd raaha
```

---

## 🎨 Step 2: Setup Frontend

### 2a. Copy source into `frontend/` folder

The React source (`src/`) needs to be inside the `frontend/` folder for the Vercel deploy.

**Windows PowerShell:**
```powershell
Copy-Item -Path "src" -Destination "frontend\src" -Recurse -Force
```

**Mac/Linux:**
```bash
cp -R src frontend/src
```

### 2b. Install frontend dependencies

```bash
cd frontend
npm install --legacy-peer-deps
```

Expected: `added 96 packages, 0 vulnerabilities`

### 2c. Setup env file

```bash
# Windows
copy .env.example .env

# Mac/Linux
cp .env.example .env
```

Edit `frontend/.env`:
```
VITE_API_URL=http://localhost:5000/api
```

### 2d. Test the frontend

```bash
npm run dev
```

Open http://localhost:5173 — should show the site!

Press `Ctrl+C` to stop.

---

## ⚙️ Step 3: Setup Backend

### 3a. Install backend dependencies

Open a **new terminal** in VS Code (`Terminal → New Terminal`):

```bash
cd backend
npm install
```

Expected: `added 250 packages, 0 vulnerabilities`

### 3b. Setup env file

```bash
# Windows
copy .env.example .env

# Mac/Linux
cp .env.example .env
```

Edit `backend/.env` — fill in real values (see below).

### 3c. Get MongoDB URI

**Free option:**
1. Go to https://cloud.mongodb.com → sign up
2. Create free M0 cluster
3. Database Access → create user (save password!)
4. Network Access → allow `0.0.0.0/0`
5. Click **Connect** → **Drivers** → copy connection string
6. Paste into `backend/.env`:
   ```
   MONGO_URI=mongodb+srv://username:PASSWORD@cluster.xxxxx.mongodb.net/khang
   ```

### 3d. Get Razorpay keys (free test mode)

1. Go to https://razorpay.com → sign up
2. Dashboard → Settings → API Keys → **Generate Test Key**
3. Copy `Key ID` and `Key Secret`
4. Paste into `backend/.env`:
   ```
   RAZORPAY_KEY_ID=rzp_test_XXXXXXXXXX
   RAZORPAY_KEY_SECRET=your_secret_here
   ```

### 3e. Get Gmail App Password

1. Enable 2FA: https://myaccount.google.com/security
2. Go to https://myaccount.google.com/apppasswords
3. Create app password for "Mail" → "Khang Backend"
4. Copy the 16-character code
5. Paste into `backend/.env`:
   ```
   EMAIL_USER=your-gmail@gmail.com
   EMAIL_PASS=abcdefghijklmnop
   RESTAURANT_EMAIL=sultham456@gmail.com
   ```

### 3f. Generate JWT secret

Generate a random string at https://randomkeygen.com → paste:
```
JWT_SECRET=paste-your-32-plus-char-random-string-here
```

### 3g. Seed the database

```bash
npm run seed
```

Output:
```
✓ Inserted 5 categories
✓ Inserted 18 products
✓ Admin created: admin@khang.com / admin123
```

### 3h. Start the backend

```bash
npm run dev
```

Output:
```
🚀 Khang API running on port 5000
🗄  MongoDB connected → cluster.xxxxx.mongodb.net/khang
💓 Health check: /health
📚 API root:     /api
```

Test in browser:
- http://localhost:5000/health → `{"status":"ok"}`
- http://localhost:5000/api/products → array of 18 dishes

---

## 🎯 Step 4: Test Full Stack

You should have **2 terminals** running:

**Terminal 1** (backend):
```bash
cd backend
npm run dev
```

**Terminal 2** (frontend):
```bash
cd frontend
npm run dev
```

Open http://localhost:5173 → try the full flow:
1. Browse menu (18 dishes should load)
2. Add items to cart
3. Sign up → check email
4. Checkout → Razorpay popup opens
5. Use test card: `4111 1111 1111 1111` / OTP `1234`
6. Payment succeeds → email arrives at `sultham456@gmail.com`

---

## 📦 Step 5: Deploy to Production

### Frontend → Vercel
```bash
# Push to GitHub first
cd ..  # back to root
git add .
git commit -m "ready to deploy"
git push origin main
```

Then:
1. Go to https://vercel.com/new → import repo
2. **Root Directory**: `frontend`
3. **Framework**: `Vite` (auto)
4. Env vars:
   - `VITE_API_URL` = your Render URL + `/api`
   - `NPM_CONFIG_LEGACY_PEER_DEPS` = `true`
5. Deploy

### Backend → Render
1. Go to https://dashboard.render.com → **New Web Service**
2. Connect repo
3. **Root Directory**: `backend`
4. **Build Command**: `npm install`
5. **Start Command**: `node src/server.js`
6. Add all env vars from `backend/.env`
7. Deploy

### Connect them
After both deploy:
- Update backend `CLIENT_URL` env var → your Vercel URL
- Update frontend `VITE_API_URL` env var → your Render URL + `/api`
- Both auto-redeploy

---

## 🐛 Common Errors & Fixes

### "'npm' is not recognized"
- Node.js not installed → download from https://nodejs.org

### "Cannot find module" errors
- Delete `node_modules/` and `package-lock.json`
- Run `npm install --legacy-peer-deps` again

### `EPERM: operation not permitted` (Windows)
- Project is in OneDrive → move to `C:\Projects\`
- Or close OneDrive sync for that folder

### Port 5000 already in use
- Change `PORT=5001` in `backend/.env`
- Or kill the process using port 5000

### MongoDB "authentication failed"
- Password in `MONGO_URI` doesn't match Atlas user
- Special chars need URL-encoding (`@` → `%40`)

### Emails not sending
- Using Gmail regular password instead of App Password
- 2FA not enabled on Gmail

### Razorpay popup shows "Invalid Key"
- Copy the full key including `rzp_test_` prefix
- Restart backend after changing `.env`

---

## ✅ Checklist

- [ ] Node.js 20+ installed
- [ ] Project cloned to `C:\Projects\raaha` (NOT OneDrive)
- [ ] Frontend `npm install` completed
- [ ] Backend `npm install` completed
- [ ] MongoDB running (local or Atlas)
- [ ] `backend/.env` filled with all keys
- [ ] `frontend/.env` has `VITE_API_URL`
- [ ] Database seeded (`npm run seed`)
- [ ] Backend running on port 5000
- [ ] Frontend running on port 5173
- [ ] Menu loads on http://localhost:5173
- [ ] Signup works and email arrives
- [ ] Test payment succeeds with Razorpay test card

---

## 📚 Related Docs

- **[frontend/README.md](./frontend/README.md)** — Frontend details
- **[backend/README.md](./backend/README.md)** — Backend API + Render deploy
- **[DEPLOYMENT.md](./DEPLOYMENT.md)** — Production deployment guide
- **[README.md](./README.md)** — Project overview
