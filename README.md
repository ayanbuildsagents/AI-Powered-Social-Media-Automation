🤖 AI Social Media Automation

An n8n automation that generates social media images with AI, sends them for approval, applies feedback, and publishes approved posts to multiple platforms.

✨ Features

🎨 Generates images using OpenAI

🖌️ Creates editable Canva designs

📧 Sends designs by email for approval

🔁 Regenerates images using reviewer feedback

📊 Uses Google Sheets as the content calendar

📁 Saves approved images to Google Drive

🚀 Publishes to LinkedIn, Facebook, and Instagram

✅ Updates post and approval status automatically

🔄 How It Works

Google Sheets
     ↓
OpenAI Image Generation
     ↓
Canva Editable Design
     ↓
Email Approval
     ↓
Approved? ── No → Update Prompt → Generate Again
     ↓ Yes
Export Image to Google Drive
     ↓
Post to Social Media
     ↓
Update Google Sheets

🛠️ Tools Used

⚙️ n8n

🤖 OpenAI API

🎨 Canva API

📊 Google Sheets

📧 Gmail

📁 Google Drive

💼 LinkedIn API

📘 Facebook Graph API

📸 Instagram Graph API

📋 Google Sheet Columns

Create a sheet named ContentPlan with these columns:

Column

Purpose

Sr No.

Post number

Date of Posting

Scheduled date

TIME OF POSTING

Scheduled time

Image Prompt

Original AI image prompt

Updated Prompt 1

First revised prompt

Updated Prompt 2

Second revised prompt

Updated Prompt 3

Third revised prompt

Processing

New, In Progress, Done, or Posted

Caption

Social media caption

Hashtags

Post hashtags

Canva_Edit_URL

Canva editing link

Approval_Attempts

Number of attempts

Posted_At

Publishing timestamp

🚀 Setup

📥 Clone this repository.

git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git

⚙️ Import workflow.json into n8n.

🔐 Connect your own credentials:

OpenAI

Google Sheets

Google Drive

Gmail

Canva

LinkedIn

Facebook and Instagram

📝 Replace all placeholder IDs:

YOUR_GOOGLE_SHEET_ID
YOUR_GOOGLE_DRIVE_FOLDER_ID
YOUR_FACEBOOK_PAGE_ID
YOUR_INSTAGRAM_ACCOUNT_ID
YOUR_REVIEW_EMAIL

🧪 Add one test row and run the workflow manually before activation.

👍 Approval Process

✅ Approve: The Canva design is exported and prepared for posting.

❌ Reject: Feedback is added to the prompt and a new image is generated.

🔁 Maximum attempts: 4 total images.

🛑 After the final rejected attempt, the workflow sends a manual-review email.

🖼️ File Naming

Final images should follow this format:

Day1.jpg
Day2.jpg
Day25.jpg

The workflow uses the day number to find the correct Google Sheets row.

⚠️ Important Checks

🔎 Confirm the workflow selects rows with the correct starting status.

🔗 Make sure all node expressions reference the correct node names.

📁 Use the same Drive folder for approved images and the publishing trigger.

🌐 Instagram requires a publicly accessible image URL.

✅ Mark a post as complete only after all required platforms publish successfully.

🔒 Security

Before publishing the repository:

🚫 Remove spreadsheet, folder, page, account, and email IDs.

🚫 Never upload API keys, tokens, passwords, or OAuth secrets.

🔐 Store credentials inside n8n.

♻️ Rotate any secret accidentally uploaded to GitHub.

📂 Repository Structure

.
├── README.md
├── workflow.json
├── .gitignore
└── LICENSE

💡 Future Improvements

✍️ Automatic caption and hashtag generation

💬 Slack or Telegram approvals

🎞️ Carousel and video-post support

📈 Post-performance tracking

🔄 Independent retry for failed platforms

🔗 Save published post URLs in Google Sheets

📄 License

Add a LICENSE file before publicly distributing the project. The MIT License is a common choice for open-source automation projects.
