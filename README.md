# LinkedIn Job Alert Automation with n8n

## 📌 Project Overview

This project automates the process of collecting the latest LinkedIn job postings using **Apify** and stores the results in **Google Sheets**. Instead of manually searching LinkedIn every day, the workflow runs automatically based on webhook input.

---

## 🚀 Features

- Accepts dynamic input through a Webhook
- Starts an Apify LinkedIn Jobs Scraper actor
- Waits until the scraping process is complete
- Handles failed actor runs gracefully
- Retrieves the scraped dataset
- Processes and formats the job data
- Saves job listings to Google Sheets
- (Bonus) Generates an AI summary of hiring trends

---

## 🛠 Tech Stack

- n8n
- Apify API
- Google Sheets API
- HTTP Request Nodes
- Webhook
- AI Agent / LLM (Bonus)

---

## 📋 Workflow

```text
Webhook
    │
    ▼
Run Apify Actor
    │
    ▼
Wait (10 Seconds)
    │
    ▼
Check Run Status
    │
    ├── SUCCEEDED ──► Get Dataset
    │
    └── RUNNING ──► Wait → Check Again
    │
    └── FAILED ──► Return Error
                     │
                     ▼
             Process Returned Data
                     │
                     ▼
            Save to Google Sheets
                     │
                     ▼
         Generate AI Summary (Bonus)
```

---

## 📥 Webhook Input

Example Request

```json
{
  "jobTitle": "Python Developer",
  "location": "Remote",
  "limit": 20
}
```

---

## 📤 Google Sheets Output

Each row contains:

- Job Title
- Company Name
- Location
- Posted Time
- Job URL
- Company URL (if available)

---

## 🔄 Workflow Logic

### 1. Webhook

Receives:

- Job Title
- Location
- Number of Jobs

### 2. Run Apify Actor

Starts the LinkedIn Jobs Scraper using the Apify API.

### 3. Wait

Pauses the workflow for 10 seconds before checking the actor status.

### 4. Check Run Status

The workflow checks whether the actor has:

- SUCCEEDED
- RUNNING
- FAILED

### 5. Loop Until Finished

If the actor is still running:

- Wait 10 seconds
- Check status again

If the actor fails:

- Return an error response

### 6. Get Dataset

Downloads the scraped LinkedIn job listings from the dataset URL returned by Apify.

### 7. Process Data

Extracts:

- Job Title
- Company
- Location
- Posted Time
- Job URL

### 8. Save to Google Sheets

Each job is appended as a new row.

### 9. AI Summary (Bonus)

Generates a short summary including:

- Most common hiring companies
- Most common locations
- Hiring trends

---

## 📌 Error Handling

The workflow automatically handles:

- Failed Apify actor runs
- Polling until the actor finishes
- Empty datasets
- Invalid webhook input

---

## 📸 Example Output

| Job Title | Company | Location | Posted Time |
|------------|----------|----------|-------------|
| Backend Engineer | Bevel | New York, NY | 2026-01-15 |
| Senior Software Engineer (Python) | Fintal Partners | New York, NY | 2026-06-30 |
| Software Engineer - Python | Canonical | Rochester, NY | 2026-03-25 |

---

## 📁 Project Structure

```
Webhook
HTTP Request (Run Actor)
Wait
HTTP Request (Check Status)
IF
IF Failed
HTTP Request (Get Dataset)
Process Returned Data
Google Sheets
AI Summary (Bonus)
```

---

## 📝 Notes

- The workflow uses only **HTTP Request** nodes to interact with Apify.
- No Apify node is used.
- Company URL is stored when available from the dataset.
- The workflow continuously polls the actor until completion.

---

## 👨‍💻 Author

Created by **Sumon Parvez**

Built as part of an **n8n AI Automation Assignment**.