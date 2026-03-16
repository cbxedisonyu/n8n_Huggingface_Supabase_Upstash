```markdown
# n8n Free Cloud Deployment Guide (Hugging Face + Supabase + Upstash)

This repository provides a guide for setting up a **completely free**, high-performance n8n environment. By combining Hugging Face's compute power, Supabase's persistent database, and Upstash's Redis cache, you can run a professional automation server at zero cost,.

## 🌟 Key Features

*   **100% Free**: No credit card or hidden fees required.
*   **Powerful Hardware**: Access **2 CPUs, 16GB RAM**, and 50GB of disk space,.
*   **Persistent Storage**: Solves the "non-persistent" issue of Hugging Face Spaces (where data is lost on restart) by using Supabase as an external database,.
*   **Optimized Sleep Policy**: Stays active for **2 days** without activity, which is significantly longer than other free tiers like Render (15 minutes).
*   **Enhanced AI Capabilities**: Integrated Upstash Redis support for superior AI Agent memory and session handling,.
*   **No Network Restrictions**: Virtually unlimited network traffic for your workflows.

---

## 🛠 Prerequisites

Before you begin, create accounts on the following platforms:
1.  **[Hugging Face](https://huggingface.co/)**: To host the n8n application.
2.  **[Supabase](https://supabase.com/)**: To host your persistent PostgreSQL database.
3.  **[Upstash](https://upstash.com/)**: To host your Redis service (optional, for AI memory).

---

## 🚀 Deployment Steps

### 1. Configure Supabase Database
1.  In Supabase, create a new **Organization** and **Project**,.
2.  **Note your Database Password** immediately; it is required for the Hugging Face setup.
3.  Set your server region (e.g., **US West - California**).
4.  Go to **Connect** settings and select **Transaction Pooler** under the SQL tab.
5.  Record the following details: `Host`, `User`, `Database name`, and `Port` (default is **6543**).

### 2. Deploy n8n on Hugging Face
1.  Go to Hugging Face **Spaces**, search for `n8n`, and find the template maintained by `bing`,.
2.  Click the three dots in the top right and select **Duplicate this Space**.
3.  Fill in the following **Environment Variables**:
    *   `DATABASE_PASSWORD`: Your Supabase password.
    *   `DATABASE_USER`: Your Supabase user.
    *   `DATABASE_HOST`: Your Supabase host address.
    *   `N8N_ENCRYPTION_KEY`: A custom key to encrypt your credentials.
    *   `N8N_PORT`: Must be set to **7860** (Hugging Face default).
4.  Set the Space to **Private** or **Public** and click **Duplicate Space** to start building.

### 3. Login and Verification
1.  Wait for the status to change to `Running`.
2.  **Important**: Check the **Logs** to find the URL ending in `.hf.space`. You must use this specific URL to access the backend login page properly.
3.  Set up your n8n owner account and **verify your email**.

### 4. Configure Upstash Redis (Optional)
1.  Create a Redis database in Upstash (Free tier includes 256MB).
2.  In n8n, create a new **Redis Credential**:
    *   `Host`: Your Upstash Endpoint.
    *   `Password`: Your Upstash Password.
    *   `Port`: **6379**.
    *   **Enable SSL** (Required).
3.  Use this in AI Agent nodes to enable `Session Time to Live`, allowing for long-term memory across sessions,.

---

## 💡 Advanced: Anti-Sleep Solution

Although Hugging Face has a generous 2-day activity window, you can ensure your instance remains active 24/7 using the following method:

1.  **Create a Health Check Table**: In your Supabase database, create an empty table named `health_check`.
2.  **GitHub Actions Workflow**: Create a workflow file in your repository (`.github/workflows/keep_alive.yml`) that runs on a cron schedule.
3.  **Periodic API Calls**: Set the workflow to periodically call a Webhook in n8n or the Supabase API to update the `health_check` table. This activity prevents the Hugging Face Space from entering sleep mode.

---

## 📅 Maintenance

*   **Updates**: To update n8n, go to the `Settings` of your Hugging Face Space and click **Factory Rebuild**. This pulls the latest Docker image (e.g., version 1.94 or latest) maintained by the template creator,.
*   **Backups**: Periodically export your workflows and credentials. This allows you to migrate to another platform instantly if needed.

---

## ⚠️ Notes

*   **Webhook URLs**: Hugging Face handles URL routing automatically. Your Webhook URLs will correctly reflect your Space's address without needing external tools like ngrok.
*   **Security**: Never share your `N8N_ENCRYPTION_KEY`. Always store sensitive passwords in the "Secret" variables on Hugging Face,.

---

**Credits**: This guide is based on the tutorial by the YouTube channel [HC AI說人話](https://www.youtube.com/@HCAI-talk).
```
