# Deployment Guide for Listen Together

This guide will help you host your own instance of the **Listen Together** application.

## Prerequisites

Before you begin, ensure you have accounts with the following services:
- **GitHub**: To host your code repository.
- **Supabase**: For the database and real-time features.
- **Vercel**: For hosting the frontend application.
- **Google Cloud Console** (Optional): For YouTube API and Google Sign-In.

---

## Step 1: Supabase Setup (Backend)

1.  **Create a Project**:
    - Go to [Supabase](https://supabase.com/) and create a new project.
    - Give it a name and secure password.

2.  **Setup Database Schema**:
    - Once the project is ready, go to the **SQL Editor** in the left sidebar.
    - Open the `schema.sql` file from this repository (located in the root folder).
    - Copy the entire content of `schema.sql` and paste it into the Supabase SQL Editor.
    - Click **Run** to create the necessary tables and policies.

3.  **Get API Credentials**:
    - Go to **Project Settings** (gear icon) -> **API**.
    - Note down the `Project URL` and `anon` / `public` key. You will need these later.

4.  **Configure Authentication** (Optional but recommended):
    - Go to **Authentication** -> **Providers**.
    - Enable **Google** if you want Google Sign-In (requires Google Cloud Console setup).
    - Ensure **Email** provider is enabled if you want email logins.
    - **IMPORTANT**: In **Authentication** -> **URL Configuration**, set the `Site URL` to your Vercel deployment URL (once you have it) or `http://localhost:3000` for local development.

---

## Step 2: Vercel Deployment (Frontend)

1.  **Import Project**:
    - Go to [Vercel](https://vercel.com/) and log in.
    - Click **Add New** -> **Project**.
    - Select your GitHub repository containing this code and click **Import**.

2.  **Configure Environment Variables**:
    - In the **Configure Project** step, expand the **Environment Variables** section.
    - Add the following variables (copy values from your Supabase project):

    | Name | Value | Description |
    | :--- | :--- | :--- |
    | `REACT_APP_SUPABASE_URL` | `Your Supabase Project URL` | From Supabase Settings -> API |
    | `REACT_APP_SUPABASE_API_KEY` | `Your Supabase Anon Key` | From Supabase Settings -> API |
    | `REACT_APP_YOUTUBE_API_KEY` | `Your YouTube Data API Key` | Get from Google Cloud Console |
    | `REACT_APP_BASE_URL` | `https://your-vercel-project.vercel.app` | Your Vercel URL (without trailing slash) |
    | `REACT_APP_SENTRY_DSN` | `(Optional)` | Leave blank if not using Sentry |

    > **Note**: For `REACT_APP_YOUTUBE_API_KEY`, you can use the one in `.env.example` temporarily, but it is highly recommended to create your own to avoid quota limits.

3.  **Deploy**:
    - Click **Deploy**.
    - Vercel will build your application. This might take a minute.
    - Once complete, you will see a "Congratulations!" screen.

---

## Step 3: Final Configuration

1.  **Update Supabase Redirect URL**:
    - After deployment, copy your new Vercel domain (e.g., `https://listen-together-xyz.vercel.app`).
    - Go back to your **Supabase Dashboard** -> **Authentication** -> **URL Configuration**.
    - Add your Vercel domain to the **Redirect URLs**.

2.  **Test Your App**:
    - Visit your Vercel URL.
    - Try signing in and creating a room.
    - If everything is set up correctly, you should be able to see the app interface!

## Troubleshooting

-   **White Screen / Errors**: Check the browser console (F12) for errors.
-   **Database Errors**: Ensure you ran the `schema.sql` script successfully in Supabase.
-   **YouTube Quota Exceeded**: You need to generate a new YouTube Data API key in Google Cloud Console and update the environment variable in Vercel.
