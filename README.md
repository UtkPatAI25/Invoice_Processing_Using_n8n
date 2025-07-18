# Invoice Processing Automation with n8n

This workflow automates the end-to-end processing of invoices received via Telegram, extracting structured data from invoices (PDF or image), storing the files in Google Drive, updating a Google Sheet, and sending a summary back to the user on Telegram. The solution leverages n8n, OpenAI (GPT-4o), Google Drive, and Google Sheets integrations for a seamless, smart, and scalable automation.

---

## Table of Contents

- [Overview](#overview)
- [Workflow Diagram](#workflow-diagram)
- [Features](#features)
- [Detailed Workflow Explanation](#detailed-workflow-explanation)
  - [1. Folder & File Preparation](#1-folder--file-preparation)
  - [2. Invoice Processing](#2-invoice-processing)
  - [3. Error Handling](#3-error-handling)
- [Sample User Experience (Telegram)](#sample-user-experience-telegram)
- [Customization](#customization)
- [Requirements](#requirements)
- [How to Import & Run](#how-to-import--run)
- [License](#license)

---

## Overview

This n8n workflow enables automatic invoice processing for businesses and freelancers. Invoices (as PDF or images) sent via Telegram are:

1. **Stored** in a year-wise folder on Google Drive.
2. **Parsed** using OpenAI (GPT-4o) for extraction of key invoice data.
3. **Logged** and updated in a Google Sheet.
4. **Summarized** and communicated back to the user on Telegram.

---

## Workflow Diagram

```
[Telegram Bot]
     |
     v
[Detect File Type]
     |
  +--+---------+
  |            |
[PDF]        [Image]
  |            |
[OpenAI GPT-4o: Extract Data]
     |
[Parse/Validate Data]
     |
[Google Drive: Store File]
     |
[Google Sheets: Update]
     |
[Telegram: Send Confirmation]
```

---

<img width="1818" height="332" alt="image" src="https://github.com/user-attachments/assets/ac5473e8-ebca-4d50-86a1-fbb0ad219b3b" />


## Features

- **Multi-format Support:** Handles both PDF and image invoices.
- **Smart Extraction:** Uses GPT-4o for accurate parsing of vendor, invoice number, dates, amounts, status, etc.
- **Drive Organization:** Auto-creates folders per year if missing; stores files in Drive with public access links.
- **Google Sheets Integration:** Updates or appends invoice data, ensuring no duplicates based on Invoice#.
- **User-Friendly:** Sends confirmation and invoice summary back on Telegram.
- **Error Handling:** Notifies user in case of extraction or processing failure.
- **Customizable:** Easily adapt fields or processing logic to your business needs.

---

## Detailed Workflow Explanation

### 1. Folder & File Preparation

- Checks if a Google Drive folder for the current year exists under the main "Invoices" directory.
- If not, creates the folder and copies a template Google Sheet ("Invoices") into it, renaming it with the year.
- Ensures an "Invoices_Summary_{YEAR}" sheet exists per year.

### 2. Invoice Processing

- Triggered by a new document sent to the Telegram bot.
- Sends an initial "processing..." message to the user.
- Downloads the file and determines its type (PDF or image).
- Extracts text or data using the appropriate OpenAI model:
  - **PDF:** Extracts text, then prompts GPT-4o to parse invoice fields.
  - **Image:** Sends image to GPT-4o Vision for document parsing.
- Parses and validates the structured data (e.g., vendor, invoice #, dates, totals).
- Uploads the original file to the correct Drive folder and shares a public link.
- Updates the corresponding Google Sheet with the extracted data, matching on Invoice# to avoid duplicates.
- Sends a summary message to the user containing parsed invoice data.

### 3. Error Handling

- If any step fails (e.g., extraction, upload, sheet update), notifies the user via Telegram with a clear error message and suggestions.

---

## Sample User Experience (Telegram)

**User:** (uploads invoice.pdf via Telegram bot)

**Bot:**
```
📄 Invoice received! Processing your document...
```
_(after a few seconds)_
```
✅ Invoice processed successfully!
📊 **Invoice Details:**
**Vendor:** ACME Corp
**Invoice #:** 12345
**Invoice Date:** 2025-07-01
**Due Date:** 2025-07-15
**Total:** $1,234.56
**Status:** Unpaid
**File Link:** [View in Google Drive](...)
```

If an error occurs:
```
❌ Processing failed
Could not extract data from document. Please ensure:
- Document is clear and readable
- Text is not rotated or upside down
- Try sending as PDF if possible
```

---

## Customization

- **Invoice Fields:** Adjust the fields in the Google Sheet mapping within the workflow to match your requirements.
- **Vendors/Statuses:** Add custom data validation or post-processing as needed.
- **Notification Message:** Edit the Telegram response templates for your tone or branding.
- **Trigger:** Replace the Telegram trigger with Email, Webhook, or another source if preferred.

---

## Requirements

- n8n (self-hosted or Cloud)
- Telegram Bot (with token)
- Google Drive & Google Sheets API credentials
- OpenAI API key (for GPT-4o)
- Access to a template Google Sheet for invoices

---

## How to Import & Run

1. **Clone or Download** this repository.
2. **Import** `Invoice_Processing.json` into your n8n workspace.
3. **Configure** credentials for Telegram, Google Drive, Google Sheets, and OpenAI in n8n.
4. **Set** your Telegram bot webhook to point to n8n if not done automatically.
5. **Customize** fields and mappings as needed.
6. **Start** the workflow and test by sending an invoice via Telegram.

---

## License

MIT License

---
