ProLogics — AI-Powered Social Media Content Automation

An end-to-end n8n workflow that turns a single row in a Google Sheet into a fully designed, human-approved social media post — generating the image with AI, building the design in Canva, routing it through an email approval loop, and (once wired up) publishing it to LinkedIn, Facebook, and Instagram.

Built by me for The Pro Accountants and Tax Consultants LLC, as a social media content-automation system.

What it does
Every day, the workflow picks up the next content row marked for processing in a Google Sheet.
It generates an image from the row's prompt using OpenAI (GPT-Image).
The image is uploaded into Canva and turned into a 1080×1080 design.
The design's edit link is emailed to the reviewer via Gmail, who approves it or leaves feedback — right from an inline form in the email.
If rejected, the feedback is merged back into the original prompt by GPT-4o-mini, a new image is generated, and the loop repeats — up to 4 attempts per post.
Once approved, the final image is exported from Canva, saved to Google Drive, and the sheet row is marked Done.
(Built, currently disabled) A second stage watches for the final uploaded image and auto-publishes it — with caption and hashtags — to LinkedIn, Facebook, and Instagram.

Schedule Trigger (daily)
        │
Get Next Row (Processing = "In Progress")
        │
Limit → 1 row per run
        │
Build sub-workflow input (day, prompt, dates, caption, hashtags)
        │
Has prompt? ──No──> (skipped)
        │ Yes
Mark row "In Progress"
        │
Generate Image (OpenAI GPT-Image) ── attempt 1, uses original prompt
        │
Upload asset → Canva → Create 1080x1080 Design
        │
Email reviewer (Gmail Send & Wait, custom form: Approve Yes/No + Feedback)
        │
   ┌────┴────┐
  Yes         No
   │           │
Export from    Merge feedback into prompt (GPT-4o-mini)
Canva (jpg)         │
   │           Under 4 attempts?
Download →          │        │
Upload to Drive     Yes       No → Email "manual review needed", stop
   │                │
Mark row "Done"   Regenerate image → new Canva design
                     │
                 Email reviewer again (loop back to Approve? check)

The approval / regeneration loop
Each rejection increments attempt_count and stores the reviewer's feedback.
Feedback is merged into the current prompt (not the original) by an LLM step, so each revision keeps everything the reviewer didn't ask to change.
Attempts 2, 3, and 4 write their prompts back to the sheet in dedicated columns (Updated Prompt 1/2/3) so there's a full audit trail of what was tried.
If attempt 4 is also rejected, the workflow stops and emails whoever's on review duty instead of looping forever — the row stays untouched until someone resets Processing back to New in the sheet.
Google Sheet schema (ContentPlan tab)
Column	Purpose
Sr No.	Day / post identifier
Date of Posting / TIME OF POSTING	Scheduled publish slot
Image Prompt	Original AI image prompt
Updated Prompt 1/2/3	Auto-filled as feedback is merged in on retries
Processing	New → In Progress → Done
Caption / Hashtags	Used at publish time
Canva_Edit_URL	Link to the final approved design
Approval_Attempts	How many rounds it took to get approved
Posted_At	Timestamp once finalized

The sheet is the single source of truth — nothing about a post's state lives only inside n8n.

Integrations / credentials required
Service	Used for
Google Sheets	Reading the content plan, writing status/prompts/attempts
Google Drive	Storing the final exported image
OpenAI (GPT-Image + GPT-4o-mini)	Image generation + feedback-to-prompt merging
Canva API (OAuth2)	Asset upload, design creation, export
Gmail (Send & Wait)	Reviewer approval emails with inline form
LinkedIn / Facebook Graph / Instagram Graph	(Stage 2, currently disabled) auto-publishing
Setup
Import the workflow JSON into n8n.
Connect credentials for Google Sheets, Google Drive, OpenAI, Canva (OAuth2), and Gmail.
Duplicate/point the ContentPlan Google Sheet to your own copy and update the documentId references.
Set the reviewer's email address in the Gmail nodes.
Add rows to the sheet with Processing = New and an Image Prompt — the daily schedule trigger will pick them up.
To enable auto-publishing, fill in the placeholder IDs (YOUR_FACEBOOK_PAGE_ID, YOUR_INSTAGRAM_BUSINESS_USER_ID, YOUR_GOOGLE_SHEET_ID, target Drive folder) in the disabled nodes, connect the LinkedIn/Facebook/Instagram credentials, and re-enable that branch.
Current status
✅ Image generation, Canva design creation, and the email approval/regeneration loop are fully built and active.
⏸️ Auto-publishing to LinkedIn, Facebook, and Instagram is built but disabled — it still has placeholder IDs and needs credentials wired in before going live.
Roadmap
Wire up and enable the publishing stage.
Replace placeholder IDs with environment-specific config.
Add retry/error alerting for the Canva export and image-generation steps.
Tech stack

n8n · OpenAI (GPT-Image, GPT-4o-mini) · Canva API · Google Sheets · Google Drive · Gmail · LinkedIn / Meta Graph API
