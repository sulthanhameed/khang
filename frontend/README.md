# 🎨 Khang Frontend

React 19 + Vite + TypeScript + Tailwind CSS v4

---

## ⚡ Quick Setup (One Time)

The `frontend/` folder needs the `src/` code copied from the repository root
because Vite requires source files next to `package.json`.

### 🖥 Windows (PowerShell):

```powershell
# From the repo root, copy src/ into frontend/
Copy-Item -Path "src" -Destination "frontend\src" -Recurse -Force
```

### 🍎 Mac/Linux:

```bash
# From the repo root, copy src/ into frontend/
cp -R src frontend/src
```

**After this**, the `frontend/` folder is fully self-contained:

```
frontend/
├── src/                    ← copied from root
│   ├── main.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── components/
│   ├── context/
│   ├── lib/
│   └── ...
├── package.json           ← has "build": "vite build"
├── vite.config.ts
├── tsconfig.json
├── index.html
├── vercel.json
├── .env.example
├── .npmrc
├── .nvmrc
└── .gitignore
```

---

## 🚀 Install & Run

### 1. Install dependencies

```bash
cd frontend
npm install --legacy-peer-deps
```

Expected output:
```
added ~96 packages, and audited 96 packages in 15s
found 0 vulnerabilities
```

### 2. Setup environment

```bash
cp .env.example .env
```

Edit `.env` and set:
```
VITE_API_URL=https://khang-backend.onrender.com/api
```

### 3. Run dev server

```bash
npm run dev
```

Opens at `http://localhost:5173`.

### 4. Build for production

```bash
npm run build
```

Creates `dist/` folder (ready for Vercel deploy).

---

## 🌐 Deploy to Vercel

### Option A: Deploy the `frontend/` folder

1. Push code to GitHub
2. Go to https://vercel.com/new → Import repo
3. Settings:
   | Field | Value |
   |---|---|
   | **Framework Preset** | `Vite` |
   | **Root Directory** | `frontend` ⚠ |
   | **Build Command** | `npm run build` |
   | **Output Directory** | `dist` |
   | **Install Command** | `npm install --legacy-peer-deps` |

4. Environment Variables:
   | Key | Value |
   |---|---|
   | `VITE_API_URL` | Your Render backend URL + `/api` |
   | `NPM_CONFIG_LEGACY_PEER_DEPS` | `true` |

5. Click **Deploy**

### Option B: Deploy from root (if source is still at root)

Same as Option A but set **Root Directory** to **blank** (uses root files).

---

## 🐛 Troubleshooting

### "Cannot find module './main.tsx'" during build
- `src/` folder is missing inside `frontend/`
- Run the copy command above (PowerShell or bash)

### "npm not recognized"
- Node.js is not installed
- Download from https://nodejs.org (LTS version 20+)

### `npm install` hangs or errors
- Make sure your project is NOT in OneDrive folder
- Move to `C:\Projects\raaha\` instead of `C:\Users\...\OneDrive\...`

### Build fails with peer dep errors
- Add `--legacy-peer-deps` flag to install
- Or add `NPM_CONFIG_LEGACY_PEER_DEPS=true` env var on Vercel

---

## 📦 What's Included

- **60+ React components** — Hero, Menu, Cart, Checkout, Admin Dashboard...
- **3 Contexts** — Auth, Cart, Theme (dark mode)
- **Razorpay integration** — real HMAC-verified payments
- **Tailwind v4** — with custom design tokens
- **TypeScript** — full type safety
- **Responsive design** — mobile-first

---

## 🔌 Backend Connection

The frontend connects to the backend via `src/lib/api.ts`:

```ts
const BASE_URL = import.meta.env.VITE_API_URL || "http://localhost:5000/api";
```

Set `VITE_API_URL` in `.env` to point to your deployed Render backend.

## 📚 See Also

- `../backend/README.md` — Backend API + Render deployment
- `../DEPLOYMENT.md` — Complete step-by-step deploy guide
- `../README.md` — Project overview
