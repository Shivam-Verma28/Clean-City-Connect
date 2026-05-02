# Deployment Guide - CleanCity Connect

This guide explains how to deploy your project to **Render** while keeping it functional locally.

## 1. Local Development (Already Working)
Your local setup uses:
- **Database**: Local PostgreSQL
- **Storage**: Local `storage/` folder
- **Auth**: Clerk (using your local keys)

**Command**: `npm run dev:web` and `npm run dev:api`

---

## 2. Production Deployment (Render)

### Step A: Prepare GitHub
Make sure your latest code is pushed to GitHub (which we just did).

### Step B: Deploy on Render
1.  Go to [Render.com](https://render.com) and log in.
2.  Click **"New +"** and select **"Blueprint"**.
3.  Connect your GitHub repository (`Clean-City-Connect`).
4.  Render will detect the `render.yaml` file. It will show:
    -   `clean-city-api` (Web Service)
    -   `clean-city-web` (Static Site)
    -   `clean-city-db` (PostgreSQL Database)
5.  Click **"Apply"**.

### Step C: Environment Variables
Once the Blueprint starts creating services, you need to add your **Clerk Keys** in the Render Dashboard:

#### For `clean-city-api`:
- `CLERK_PUBLISHABLE_KEY`: (From Clerk Dashboard)
- `CLERK_SECRET_KEY`: (From Clerk Dashboard)

#### For `clean-city-web`:
- `VITE_CLERK_PUBLISHABLE_KEY`: (Same as above)
- `VITE_API_URL`: (The URL of your `clean-city-api` service, e.g., `https://clean-city-api.onrender.com`)

---

## 3. Important Notes for Production

### Database Migration
The Blueprint creates a fresh database. You might need to run your Drizzle migrations:
`pnpm -C lib/db drizzle-kit push` (or similar) from the Render dashboard console if needed.

### Storage in Production
Currently, the API is set to use local storage. **Warning**: On Render Free tier, files uploaded to the server will be deleted when the server restarts.
**Solution**: 
1. Set up a **Google Cloud Storage** bucket.
2. Add your Google credentials to the API environment variables.
3. The app is already coded to use GCS if the keys are present.

### Authentication (Clerk)
Make sure you add your Production URL (e.g., `https://clean-city-web.onrender.com`) to the **Allowed Origins** in your Clerk Dashboard settings.
