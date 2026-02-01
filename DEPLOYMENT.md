# Deploying SteganoGuard to Render

This guide will help you host your Flask application on Render with a PostgreSQL database.

## Prerequisites
- A GitHub account.
- A [Render](https://render.com) account.

## Step 1: Push Code to GitHub

1.  Initialize git in your project folder (if not done):
    ```bash
    git init
    # The .gitignore file I created will ensure .env is NOT uploaded
    git add .
    git commit -m "Initial commit"
    ```
2.  Create a new repository on GitHub.
3.  Push your code:
    ```bash
    git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
    git branch -M main
    git push -u origin main
    ```

## Step 2: Create a Database on Render

1.  Log in to your Render Dashboard.
2.  Click **New +** -> **PostgreSQL**.
3.  **Name**: `steganoguard-db` (or any name).
4.  **Region**: Choose the one closest to you (e.g., Singapore).
5.  **Instance Type**: Select **Free**.
6.  Click **Create Database**.
7.  **IMPORTANT**: Once created, look for the **"Internal Database URL"**. Copy this URL. You will need it soon.

## Step 3: Create the Web Service

1.  Go back to the Render Dashboard.
2.  Click **New +** -> **Web Service**.
3.  Select **Build and deploy from a Git repository**.
4.  Connect your GitHub account and select your `SteganoGuard` repository.
5.  **Name**: `steganoguard-app` (this will be your URL: `steganoguard-app.onrender.com`).
6.  **Region**: Same as your database.
7.  **Branch**: `main`.
8.  **Runtime**: `Python 3`.
9.  **Build Command**: `pip install -r requirements.txt`.
10. **Start Command**: `gunicorn app:app` (This is already in your `Procfile`, but good to double-check).
11. **Instance Type**: Select **Free**.

## Step 4: Configure Environment Variables

Scroll down to the **Environment Variables** section and click **Add Environment Variable**. Add the following:

| Key | Value |
| :--- | :--- |
| `DATABASE_URL` | Paste the **Internal Database URL** you copied in Step 2. |
| `FLASK_SECRET_KEY` | Generate a random string (e.g., `supersecretkey123`). |
| `PYTHON_VERSION` | `3.10.0` (Optional, but good for stability). |

**Note**: You do **NOT** need to add `DB_USER`, `DB_PASSWORD`, etc., separately because Render's `DATABASE_URL` contains all that info in one string, and our `app.py` is programmed to read it.

## Step 5: Deploy

1.  Click **Create Web Service**.
2.  Render will verify the code and start building. This might take a few minutes.
3.  Watch the logs. If you see "Build successful" and "Starting service with gunicorn", you are live!
4.  Click the URL at the top cleanly (e.g., `https://steganoguard-app.onrender.com`) to visit your site.

## Troubleshooting

-   **Database Errors**: Ensure you copied the *Internal* Database URL, not the External one (unless connecting from your own PC).
-   **Build Failed**: Check the logs. Usually, it's a missing library in `requirements.txt`.
-   **App Crashes**: Check the logs for `ImportError` or `KeyError`.

---
**Good Luck!** Your app is ready for the world.
