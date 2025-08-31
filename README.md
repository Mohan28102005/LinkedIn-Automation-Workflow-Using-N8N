# LinkedIn Automation Workflow

This project is an **n8n automation workflow** that automatically pulls
the latest drone and UAV-related news from Google News RSS, summarizes
it using AI, and posts it to LinkedIn with an accompanying image. The
workflow also tracks everything in Google Sheets for record-keeping.

------------------------------------------------------------------------

## 🔄 Workflow Overview

1.  **Trigger**
    -   Runs daily at **11:30 PM (cron expression: `30 23 * * *`)**.
2.  **Fetch News Articles**
    -   Uses an **HTTP Request** node to pull news from Google News RSS
        feed with the query:

            drone OR UAV technology OR DGCA India (past 1 day)

    -   Converts RSS feed into JSON format.
3.  **Process Articles**
    -   Extracts the top 5 latest articles.
    -   Captures:
        -   Title
        -   Link
        -   Publication Date
        -   Image (or assigns a placeholder if missing)
4.  **Summarize with AI**
    -   An **AI Agent (OpenRouter GPT OSS 12B)** writes a professional
        1-paragraph summary.
    -   Adds **3 relevant hashtags** for LinkedIn.
    -   Keeps the summary under **800 characters**.
5.  **Image Handling**
    -   If no image is found in the RSS, the workflow queries **Tavily
        Search API** for a relevant image.
    -   Filters for **.jpg/.jpeg** images and uses the first result.
6.  **Data Storage**
    -   Appends each article (title, link, summary, publication date,
        image, status) to a **Google Sheet**.
    -   Marks rows as `Done = false` initially.
    -   After posting, updates `Done = true`.
7.  **LinkedIn Posting**
    -   Downloads the article image.
    -   Creates a **LinkedIn post** with:
        -   Title
        -   AI-generated summary
        -   Link for full article
        -   Image
8.  **Spreadsheet Update**
    -   Once posted, marks the corresponding row as **Done** to prevent
        duplicates.

------------------------------------------------------------------------

## 📊 Google Sheets Integration

-   Sheet stores:
    -   `title`
    -   `link`
    -   `summary`
    -   `pubDate`
    -   `Image`
    -   `Done` (boolean tracker)

This makes it easy to audit which articles have been posted and when.

------------------------------------------------------------------------

## 💡 Key Features

-   **Completely automated LinkedIn content pipeline**.
-   **AI-powered summaries & hashtags** for professional posts.
-   **Failsafe image handling** (RSS → Tavily fallback).
-   **Google Sheets tracking** for transparency & debugging.
-   **Non-duplicate posting system** using the `Done` flag.

------------------------------------------------------------------------

## 🚀 Usage

### 1. Import Workflow

-   Open **n8n**.
-   Import the provided JSON workflow file
    (`LinkedIn Automation Workflow.json`).

### 2. Connect Accounts

-   **Google Sheets**
-   **LinkedIn**
-   **OpenRouter (AI model)**
-   **Tavily (search + images)**

### 3. Configure Settings

-   Update your:
    -   Google Sheet ID
    -   LinkedIn Profile ID
    -   API credentials for integrations

### 4. Activate Workflow

-   Enable the workflow in **n8n**.
-   LinkedIn posts will now be created automatically every day.

------------------------------------------------------------------------

## 🛠️ Tech Stack

-   [n8n](https://n8n.io) -- Workflow automation
-   [Google Sheets API](https://developers.google.com/sheets/api) --
    Data storage
-   [LinkedIn
    API](https://learn.microsoft.com/en-us/linkedin/marketing/) -- Post
    publishing
-   [OpenRouter](https://openrouter.ai) -- AI-powered summaries
-   [Tavily](https://tavily.com) -- Image fetching

------------------------------------------------------------------------

## 📌 License

This project is licensed under the MIT License -- feel free to use,
modify, and share.

------------------------------------------------------------------------

## 🤝 Contributions

Pull requests are welcome! For major changes, please open an issue first
to discuss what you'd like to change.
