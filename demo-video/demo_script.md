# Demo Video Script — Lead Automation Workflow

**Duration:** 3–5 minutes  
**Format:** Screen recording only (no face required)  
**Watch:** [Lead Automation Demo on Google Drive](https://drive.google.com/file/d/1jTqRmZyaEI6GLbaSTA4uZdTyfU998jhe/view?usp=sharing)

---

## Pre-Recording Checklist

- [ ] n8n workflow **automation leads** is open in Editor tab  
- [ ] Google Sheet **Lead Database** is open in a browser tab  
- [ ] Gmail inbox is open (logged in as test recipient)  
- [ ] Postman, curl, or n8n **Test workflow** ready with sample payload  
- [ ] Close unrelated tabs and notifications  
- [ ] Set display zoom to 100–125% for readability  

---

## Scene 1 — Introduction (0:00–0:30)

**[Screen: n8n workflow canvas — all three nodes visible]**

**Narration (optional voiceover or on-screen text):**

> "This is my Lead Automation workflow for the Earthonoid AI internship assessment. It captures leads via webhook, saves them to Google Sheets, and sends a Gmail confirmation automatically."

**Actions:**

- Pan across: **Webhook → Append row in sheet → Send a message**
- Briefly highlight workflow name: **automation leads**

---

## Scene 2 — Workflow Overview (0:30–1:15)

**[Screen: Click each node to show configuration — do not expose secrets]**

**Webhook node:**

- Show **POST** method and path **`lead-form`**
- Mention it accepts JSON lead data

**Google Sheets node:**

- Show operation **Append row**
- Show spreadsheet **Lead Database** and columns: name, email, phone no

**Gmail node:**

- Show subject: **Lead Registration Confirmation**
- Show recipient expression uses lead email
- Scroll message body — mention Earthonoid sign-off

---

## Scene 3 — Webhook Trigger (1:15–2:00)

**[Screen: Postman or terminal with curl]**

**Sample payload (read or paste on screen):**

```json
{
  "Name": "Vaibhav",
  "Email": "vkkanojia079@gmail.com",
  "Phone": "9876543210"
}
```

**Actions:**

- Send **POST** to production or test webhook URL  
  **OR** click **Execute workflow** in n8n with pinned test data  
- Switch to n8n and show execution starting (loading → running)

**Narration:**

> "I'm triggering the workflow with a sample lead payload, simulating a form submission."

---

## Scene 4 — Google Sheets Update (2:00–2:45)

**[Screen: Google Sheet — Lead Database]**

**Actions:**

- Refresh the sheet
- Point to the **new row** with name, email, and phone
- Highlight column headers: name, email, phone no

**Narration:**

> "The lead was appended automatically—no manual entry required."

---

## Scene 5 — Gmail Confirmation (2:45–3:30)

**[Screen: Gmail inbox]**

**Actions:**

- Open email **Lead Registration Confirmation**
- Show recipient, subject, and body text
- Briefly show n8n footer (proves automation origin)

**Narration:**

> "The lead receives an immediate confirmation email from our automated workflow."

---

## Scene 6 — Successful Execution (3:30–4:15)

**[Screen: n8n — Executions tab or canvas after run]**

**Actions:**

- Show **green checkmarks** on all three nodes
- Show **1 item** on each connection (if visible on canvas)
- Open **Executions** list → latest run → **Succeeded**
- Optional: open execution detail showing each node output

**Narration:**

> "All nodes completed successfully—webhook, Google Sheets, and Gmail."

---

## Scene 7 — Closing (4:15–4:45)

**[Screen: README on GitHub or repo folder structure]**

**Narration:**

> "The full workflow export, screenshots, and documentation are in my GitHub repository. Thank you for reviewing my submission."

**On-screen text (optional):**

- Repository name  
- Your name  
- Earthonoid AI Automation Internship — Question 1  

---

## Recording Tips

1. Use **OBS Studio** or **Windows Game Bar** (Win + G).  
2. Record at **1080p**; export as **MP4 (H.264)**.  
3. Move mouse slowly; pause 2 seconds on each proof screen.  
4. Do not show OAuth tokens, webhook secrets, or full API keys.  
5. If webhook URL is visible, blur it in post-production or use test URL only.  

---

## Video Verification Checklist (for reviewers)

| Requirement | Timestamp (fill after recording) |
|-------------|----------------------------------|
| Workflow overview | |
| Webhook trigger | |
| Google Sheets update | |
| Gmail confirmation | |
| Successful execution | |

---

*Demo recording:* [Google Drive link](https://drive.google.com/file/d/1jTqRmZyaEI6GLbaSTA4uZdTyfU998jhe/view?usp=sharing)
