# WhatsApp_Automation 🚀🚀
A WhatsApp AI bot for students. Handles text, voice, and image messages, fetches course materials from Moodle, searches the web for answers, and manages Google Calendar events (create, view, update) — all powered by n8n and multiple LLMs (Gemini, Openai) and Openrouter.

> ⚠️ **Important:** You need an [OpenRouter](https://openrouter.ai) account with sufficient credits to run the Chat Model, and a free [Gemini](https://aistudio.google.com/) account to use the remaining features.

## 📁 Versions

| File | Description |
|---|---|
| `WhatsApp_ChatBot_V1.json` | Core bot: text, voice and image messages, Moodle files, web search, Google Calendar. |
| `WhatsApp_ChatBot_V2.json` | Everything in V1, plus Zajel attendance, Telegram course materials, long video analysis, and LinkedIn profile analysis. |

# ✅ Version 1 Features

### 💬 Text, Voice & Image Messages
- **Text:** chat with the AI agent in Arabic or English.
- **Voice notes:** the bot transcribes your voice message with Gemini and can reply with a voice note (text-to-speech).
- **Images:** send a photo and the bot analyzes it and answers your question about it.

### 📚 Moodle Files
- List the PDF files from all your Moodle courses, grouped by course and sorted from newest to oldest.
- Ask for one or several files by number and the bot sends you the links.

### 🔍 Web Search
Ask any general question and the bot searches the web (SerpApi) and answers in a short, clear way with sources.

### 📅 Google Calendar
Book, view, update, and delete appointments using natural language. The bot checks for conflicts and past dates before saving, and always uses Palestine time.

# 🎬 Automation Video Tutorial

💡 Before downloading the JSON file, please make sure to set up n8n on your device and check out the tutorial resources below:

* 📺 **n8n Installation Guide (5 mins):** [Watch Video Guide](https://youtu.be/lr7b37quIds?si=uSsupLkmZ-hJvm4w)
* 🎥 **Full Automation Guide Video (27 mins):** [Watch on Google Drive](https://drive.google.com/file/d/1yWorQOgykSGqrEzD8_m5zbi7X9Mbq8Q1/view?usp=drive_link)
* 💻 **Just Practical Application Video (3 mins):** [Watch on Google Drive](https://drive.google.com/file/d/10UzWhHQvy7bdB7kVaLa846oRwCbmkuGJ/view?usp=drive_link)

# 🆕 Version 2 Features (New)

### 🕒 Zajel Attendance & Absence
Ask the bot for your attendance and absence and it logs in to Zajel, fetches your data, and replies with a clean per-course summary: total absence hours, unexcused vs. excused hours, and the remaining allowed hours.
> Example: "show me my absences"

### 🎓 Telegram Course Materials
The bot talks to a dedicated Telegram bot that stores materials for Computer Engineering courses, and fetches the files you ask for: **summaries, past exams (quizzes, midterms, finals), and course projects**.
- Students often use nicknames for courses, so the bot first resolves the nickname to the official course name using a Google Sheet (`Course Name` / `Nickname` columns).
- It then finds the matching path in the Telegram bot and returns the files.
- If a course has no projects or files, the bot tells you in the language you wrote in.
> Example: "send me digital 1 summaries"

### 🤖 Telegram Bridge (`server.js`)
A small Node.js server (Express + GramJS) logs in to Telegram as a regular user and talks to the course materials bot on the student's behalf.
- `GET /tree` crawls all menus of the Telegram bot and saves every path (e.g. `Courses > Digital 1 > Summaries`) to `bot_tree.json`.
- `GET /paths` returns the saved paths, used by the `find_telegram_paths` tool.
- `POST /run` follows a path by pressing the buttons in order and returns the files, used by the `get_telegram_files` tool.
- Requests are processed one at a time (a lock) because the Telegram bot keeps the menu state of the conversation.

### 🎬 Long Video Analysis (15–20 minutes)
Send a video and the bot analyzes it with Gemini, then replies with a short summary, key points, and a takeaway. It works well for lectures and tutorials.
> Note: WhatsApp Cloud API limits media files to about 16 MB, so very long videos must be compressed or low quality to be accepted.

### 💼 LinkedIn Profile Analysis
A dedicated LinkedIn sub-agent reviews your profile and recent posts and returns only what needs improvement: headline, about, experience, skills, projects, and post writing style. It finishes with your **Top 3 Priorities**. Replies for LinkedIn requests are always in English.
- Profile and posts are scraped with [Apify](https://apify.com) actors (`harvestapi/linkedin-profile-scraper` and `harvestapi/linkedin-profile-posts`).
- Replies are kept under WhatsApp's 4096-character limit.

### ⚙️ Other Improvements
- **~30% faster** automation: unnecessary waiting messages are skipped, and Meta's status events are filtered out before they reach the agent.
- **~15% fewer tokens** used per request, thanks to a cleaner, more compact system prompt and smaller tool outputs.
- **Improved error handling:** errors were reduced by ~20%, and every failure now returns a clear message in the user's language.
- **~35% higher productivity** for bot users: students get attendance, course materials, video summaries, and LinkedIn feedback in one chat, instead of switching between Zajel, Moodle, Telegram, and other tools.

# 🔑 Setup for Version 2

The exported JSON contains **no credentials or secrets**. After importing it into n8n:

1. Create and connect these credentials: WhatsApp (Trigger and Send), OpenRouter, Google Gemini, Google Calendar, Google Sheets, SerpApi, and ElevenLabs.
2. Fill in your own values in the **Text Processing** node (Moodle token and Zajel login). Never commit real values to GitHub.
3. Set your Apps Script webhook URL in the Moodle tools (`YOUR_APPS_SCRIPT_WEBHOOK_URL`).
4. Add your Apify token in the two LinkedIn tools (`YOUR_APIFY_TOKEN`) and your own LinkedIn profile URL.
5. Connect your Telegram bot tools and the course names Google Sheet.
6. Activate the workflow and send "Hi" to your WhatsApp number.

# 🎬 Automation Video Tutorial

💡 Before downloading the JSON file, please make sure to set up n8n on your device and check out the tutorial resources below:
* 📺 **Version 2 Guide :** [Watch Video Guide](https://drive.google.com/drive/folders/1JVfkZWJe1-JDFFtrsPolUp5CZDldUIM5?usp=drive_link)
