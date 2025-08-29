# Telegram AI Article Summarizer Bot

A powerful Telegram bot that can read any article link and provide a concise, AI-powered summary. This project is built using n8n and leverages the Dobby model from SentientAGI to distill long-form content into key points.

**Bot Link:** [https://t.me/Sentientagi_summerize_bot](https://t.me/Sentientagi_summerize_bot)

---

## Features

-   **Summarize Any Link:** Simply send the bot a URL to an article, and it will read and summarize it.
-   **Advanced Web Scraping:** Intelligently scrapes and cleans the HTML from the source to extract clean article text.
-   **AI-Powered Summaries:** Uses the Dobby AI model to generate high-quality, human-readable summaries.
-   **Handles Long Content:** Automatically splits long summaries into multiple messages to respect Telegram's character limits.
-   **Rate Limiting:** Manages user access with a limit of 3 summaries per day, tracked in Google Sheets.

## How It Works

1.  **Receive Link:** The n8n workflow is triggered when a user sends a message containing a URL to the Telegram bot.
2.  **Rate Limit Check:** The workflow looks up the user's ID in a Google Sheet to verify they haven't exceeded their daily limit.
3.  **Extract & Scrape:** The bot extracts the URL from the message, then makes an HTTP request to scrape the raw HTML of the webpage.
4.  **Clean & Sanitize:** A custom Code node executes a complex script to strip all HTML tags, scripts, and styles, converting the page into clean, readable plain text.
5.  **AI Summarization:** The clean text is sent to the Dobby AI model with the prompt: "You are an expert summarizer... provide a concise and clear summary."
6.  **Split for Telegram:** The potentially long summary from the AI is processed by another Code node, which intelligently splits it into chunks that fit within Telegram's message length limits.
7.  **Send Summary:** Each chunk is sent back to the user in a separate Telegram message.

## Setup Instructions

To run this workflow yourself:

1.  **Download:** Download the workflow JSON file from this repository.
2.  **Import:** Import the JSON file into your n8n instance.
3.  **Credentials:** Connect your own credentials for:
    *   Telegram
    *   Google Sheets
    *   Fireworks AI (using HTTP Header Auth)
4.  **Google Sheet:** Create a new Google Sheet with three columns: `UserID`, `MessageCount`, and `Date`. Update the Google Sheets nodes in the workflow to point to your new sheet.
5.  **Activate:** Activate the workflow.

## Tech Stack

-   **Automation:** [n8n.io](https://n8n.io/)
-   **AI Model:** SentientAGI's `dobby-mini-unhinged-plus-llama-3-1-8b` via Fireworks AI
-   **Web Scraping & Cleaning:** Custom JavaScript within n8n
-   **Database:** Google Sheets
