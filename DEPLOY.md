# XYZ School — Deployment Guide (Render + Vercel)

## Overview
- **Backend** → Deploy on [Render](https://render.com) (Node.js/Express)
- **Frontend** → Deploy on [Vercel](https://vercel.com) (React/Vite)

---

## Step 1: Push to GitHub

1. Create two GitHub repositories (or one monorepo):
   - `xyz-school-backend`
   - `xyz-school-frontend`

2. Push the `backend` folder to the backend repo.
3. Push the `frontend` folder to the frontend repo.

> ⚠️ Make sure `.env` is **NOT** committed (it's in `.gitignore`).

---

## Step 2: Deploy Backend on Render

1. Go to [render.com](https://render.com) → **New → Web Service**
2. Connect your GitHub backend repository
3. Configure:
   - **Name:** `xyz-school-api`
   - **Root Directory:** *(leave blank if repo is just the backend folder)*
   - **Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`

4. Add Environment Variables (in Render dashboard):

   | Key | Value |
   |-----|-------|
   | `PORT` | `5000` |
   | `NODE_ENV` | `production` |
   | `MONGO_URI` | your MongoDB Atlas URI |
   | `JWT_SECRET` | a long random secret string |
   | `JWT_EXPIRE` | `1h` |
   | `CLOUDINARY_CLOUD_NAME` | your cloudinary name |
   | `CLOUDINARY_API_KEY` | your cloudinary key |
   | `CLOUDINARY_API_SECRET` | your cloudinary secret |
   | `FRONTEND_URL` | *(add after Vercel deploy — your Vercel URL)* |

5. Click **Deploy** — note the URL (e.g., `https://xyz-school-api.onrender.com`)

---

## Step 3: Deploy Frontend on Vercel

1. Go to [vercel.com](https://vercel.com) → **New Project**
2. Import your GitHub frontend repository
3. Configure:
   - **Framework Preset:** Vite
   - **Root Directory:** *(leave blank if repo is just the frontend folder)*
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`

4. Add Environment Variables (in Vercel dashboard → Settings → Environment Variables):

   | Key | Value |
   |-----|-------|
   | `VITE_API_URL` | `https://xyz-school-api.onrender.com/api` |

5. Click **Deploy** — note your Vercel URL (e.g., `https://xyz-school.vercel.app`)

---

## Step 4: Update CORS on Backend

After Vercel deployment, go back to Render and add/update:

| Key | Value |
|-----|-------|
| `FRONTEND_URL` | `https://xyz-school.vercel.app` |

Then redeploy the backend (or it auto-redeploys on env change).

---

## Step 5: Update School Settings (Admin Panel)

After deployment, log in as Admin and go to **Settings** to update:
- School Name: `XYZ School`
- School Address
- Contact Number
- School Logo
- Academic Year

---

## Notes

- Render free tier **spins down** after 15 minutes of inactivity — first request may take ~30s to wake up.
- MongoDB Atlas: Make sure your cluster allows connections from `0.0.0.0/0` (all IPs) for Render.
- The `vercel.json` file in the frontend handles React Router's client-side routing.
