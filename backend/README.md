# ⚙️ Khang Backend

Express + MongoDB + Razorpay REST API

---

## 🚀 Install & Run

### 1. Install dependencies

```bash
cd backend
npm install
```

Expected output:
```
added ~250 packages, and audited 251 packages in 20s
found 0 vulnerabilities
```

### 2. Setup MongoDB

**Option A — Local (Docker):**
```bash
docker run -d --name khang-mongo -p 27017:27017 mongo:7
```

**Option B — MongoDB Atlas (free cloud):**
1. Go to https://cloud.mongodb.com
2. Create free M0 cluster
3. Get connection string

### 3. Setup environment

```bash
cp .env.example .env
```

Edit `.env` with your values:
```env
NODE_ENV=production
PORT=5000
CLIENT_URL=https://khang.vercel.app

MONGO_URI=mongodb+srv://khang:PASSWORD@cluster.mongodb.net/khang

JWT_SECRET=your-random-32-char-string
JWT_EXPIRES_IN=7d

RAZORPAY_KEY_ID=rzp_test_XXXX
RAZORPAY_KEY_SECRET=your_razorpay_secret

EMAIL_USER=your-gmail@gmail.com
EMAIL_PASS=your-16-char-app-password
RESTAURANT_EMAIL=sultham456@gmail.com
```

### 4. Seed the database

```bash
npm run seed
```

Output:
```
✓ Inserted 5 categories
✓ Inserted 18 products
✓ Admin created: admin@khang.com / admin123
```

### 5. Start dev server

```bash
npm run dev
```

Output:
```
🚀 Khang API running on port 5000
🗄  MongoDB connected → khang
💓 Health check: /health
📚 API root:     /api
```

### 6. Test

Open browser:
- `http://localhost:5000/health` → returns `{"status":"ok"}`
- `http://localhost:5000/api/products` → returns 18 dishes

---

## 🌐 Deploy to Render

### Step 1: Push code to GitHub

```bash
git add .
git commit -m "backend ready"
git push origin main
```

### Step 2: Create Render Web Service

1. Go to https://dashboard.render.com → **New +** → **Web Service**
2. Connect your GitHub repo
3. Configure:

| Field | Value |
|---|---|
| **Name** | `khang-backend` |
| **Region** | `Singapore` |
| **Branch** | `main` |
| **Root Directory** | `backend` ⚠ IMPORTANT |
| **Runtime** | `Node` |
| **Build Command** | `npm install` |
| **Start Command** | `node src/server.js` |
| **Plan** | `Free` |

### Step 3: Add Environment Variables

Add all keys from `.env.example` in Render's Environment tab.

### Step 4: Deploy

Click **Create Web Service** → wait ~2 min → get your URL:
```
https://khang-backend.onrender.com
```

### Step 5: Seed on Render

Open **Shell** tab in Render → run:
```bash
node src/scripts/seed.js
```

---

## 📁 Structure

```
backend/
├── src/
│   ├── server.js              # Express bootstrap
│   ├── config/db.js           # MongoDB connection
│   ├── models/                # Mongoose schemas
│   ├── controllers/           # Business logic
│   ├── routes/                # REST endpoints
│   ├── middleware/            # JWT auth + errors
│   ├── utils/                 # Email + JWT helpers
│   └── scripts/seed.js        # DB seeding
├── package.json               # scripts: start, dev, seed
├── render.yaml                # 1-click Render config
├── Dockerfile
├── .env.example
├── .gitignore
└── README.md
```

---

## 📡 API Endpoints

### Auth
- `POST /api/auth/signup` — Create account
- `POST /api/auth/login` — Login (returns JWT)
- `GET /api/auth/me` — Get current user

### Products
- `GET /api/products` — List all
- `GET /api/products/:slug` — Get one + reviews

### Orders
- `POST /api/orders` — Create order (auth required)
- `GET /api/orders/track/:orderId` — Public tracking
- `GET /api/orders` — Admin: list all
- `PUT /api/orders/:id/status` — Admin: update status

### Payments (Razorpay)
- `POST /api/payments/razorpay/create-order` — Create Razorpay order
- `POST /api/payments/razorpay/verify` — Verify HMAC signature

### Reviews
- `GET /api/reviews?product=slug` — List reviews
- `POST /api/reviews` — Create review

---

## 🔐 Environment Variables (Required)

| Key | Description | Example |
|---|---|---|
| `NODE_ENV` | Environment | `production` |
| `PORT` | Server port | `5000` |
| `CLIENT_URL` | Frontend URL (CORS) | `https://khang.vercel.app` |
| `MONGO_URI` | MongoDB connection | `mongodb+srv://...` |
| `JWT_SECRET` | JWT signing secret | Random 32+ char string |
| `JWT_EXPIRES_IN` | Token lifetime | `7d` |
| `RAZORPAY_KEY_ID` | Razorpay public key | `rzp_test_XXXX` |
| `RAZORPAY_KEY_SECRET` | Razorpay secret | (from dashboard) |
| `EMAIL_USER` | Gmail address | `your@gmail.com` |
| `EMAIL_PASS` | Gmail App Password | 16-char code |
| `RESTAURANT_EMAIL` | Restaurant email | `sultham456@gmail.com` |

---

## 🐛 Troubleshooting

### "MongoServerError: bad auth"
- Password in `MONGO_URI` doesn't match Atlas user
- URL-encode special chars in password (`@` → `%40`)

### "Razorpay keys missing"
- Set `RAZORPAY_KEY_ID` and `RAZORPAY_KEY_SECRET` env vars
- After adding, click **Save Changes** in Render to redeploy

### CORS error in browser
- Backend `CLIENT_URL` doesn't match your Vercel URL
- No trailing slash: `https://khang.vercel.app` (not `.../`)

### Emails not sending
- Use Gmail **App Password** (not regular password)
- Enable 2FA on Gmail first: https://myaccount.google.com/security
- Get App Password: https://myaccount.google.com/apppasswords

### Render free tier sleeps
- Free plan sleeps after 15 min idle
- First request takes ~30 sec cold start
- Upgrade to $7/mo Starter for always-on

---

## 🛡 Security

- ✅ **bcryptjs** — passwords hashed (10 rounds)
- ✅ **JWT** — signed with HS256, 7-day expiry
- ✅ **Razorpay HMAC-SHA256** — every payment verified
- ✅ **Helmet** — HTTP security headers
- ✅ **CORS** — restricted to your frontend
- ✅ **Rate limiting** — 300 req / 15min per IP
- ✅ **Server-side price calc** — no client tampering

---

## 📚 See Also

- `../frontend/README.md` — Frontend setup
- `../DEPLOYMENT.md` — Complete deploy guide
- `../README.md` — Project overview
