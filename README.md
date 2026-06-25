<![CDATA[# 📰 News Generator — RPA Automation

> **Automatically fetch the latest news on any topic and deliver it straight to your inbox — powered by UiPath.**

![Platform](https://img.shields.io/badge/Platform-UiPath-orange?style=flat-square)
![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=flat-square)
![Framework](https://img.shields.io/badge/Target-Windows-green?style=flat-square)
![Studio](https://img.shields.io/badge/Studio-26.0-purple?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [How It Works](#-how-it-works)
- [Architecture](#-architecture)
- [Tech Stack & Dependencies](#-tech-stack--dependencies)
- [Prerequisites](#-prerequisites)
- [Installation & Setup](#-installation--setup)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [Contact](#-contact)

---

## 🎯 About the Project

**News Generator** is a UiPath Robotic Process Automation (RPA) bot that eliminates the manual effort of searching for the latest news. Simply provide a topic and an email address — the bot will:

- Search Google News for the most recent articles on your topic
- Validate that the results are fresh (posted within hours, not days)
- Extract the top headline and its source URL
- Compose and send a formatted email with the news directly to your inbox

This is ideal for professionals, researchers, and teams who need a quick daily briefing on specific subjects without manually browsing multiple news sources.

---

## ⚙️ How It Works

The automation follows a streamlined six-step workflow:

| Step | Action | Details |
|:----:|--------|---------|
| **1** | 🖊️ **User Input** | Bot prompts for a search topic and recipient email address via input dialogs |
| **2** | 🌐 **Open Browser** | Launches Microsoft Edge and navigates to Google |
| **3** | 🔍 **Search & Navigate** | Enters the topic in the Google search bar and clicks the **"News"** tab |
| **4** | 📰 **Extract Data** | Scrapes the latest news headlines and their corresponding URLs |
| **5** | ✅ **Validate Freshness** | Checks if the top article timestamp contains **"hours"** to ensure recency |
| **6** | 📧 **Send Email** | Retrieves SMTP credentials from Orchestrator and sends the news via Gmail |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      USER INPUT                         │
│              (Topic + Email Address)                    │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│                  MICROSOFT EDGE                         │
│          Google Search → News Tab                       │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│                DATA EXTRACTION                          │
│       Headlines, URLs, Timestamps                       │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│              FRESHNESS VALIDATION                       │
│      Is the article posted within hours?                │
└──────────┬───────────────────────────────┬──────────────┘
           │ YES                           │ NO
           ▼                               ▼
┌─────────────────────┐      ┌────────────────────────────┐
│   📧 SEND EMAIL     │      │  ⚠️ LOG WARNING            │
│  via Gmail SMTP     │      │  No recent news found      │
└─────────────────────┘      └────────────────────────────┘
```

---

## 🛠️ Tech Stack & Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `UiPath.System.Activities` | `25.12.2` | Core system activities (Input Dialog, Assign, If, etc.) |
| `UiPath.UIAutomation.Activities` | `25.10.26` | Browser automation (Click, Type Into, Data Scraping) |
| `UiPath.Mail.Activities` | `2.7.0-preview` | SMTP email sending via Gmail |

**Runtime Configuration:**
- **Expression Language:** Visual Basic
- **Target Framework:** Windows
- **Serialization:** NewtonsoftJson
- **Execution Type:** Workflow (Pausable)

---

## ✅ Prerequisites

Before running this project, ensure you have the following:

1. **UiPath Studio** `v26.0+` installed on your machine
2. **Microsoft Edge** browser installed and updated
3. **Google Account** with an [App Password](https://myaccount.google.com/apppasswords) generated for SMTP access
4. **UiPath Orchestrator** access (local folder or cloud) with a credential asset configured

### Orchestrator Asset Setup

Create a **Credential** asset in UiPath Orchestrator:

| Property | Value |
|----------|-------|
| **Asset Name** | `Sender_Gmail` |
| **Asset Type** | Credential |
| **Username** | Your Gmail address (e.g., `yourname@gmail.com`) |
| **Password** | Your Gmail [App Password](https://support.google.com/accounts/answer/185833) (16-character code) |

> ⚠️ **Important:** Do **not** use your regular Gmail password. You must generate a dedicated App Password from your Google Account security settings with 2-Step Verification enabled.

---

## 🚀 Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/News-Generator.git
cd News-Generator
```

### 2. Open in UiPath Studio

- Launch **UiPath Studio**
- Click **Open Project** → navigate to the cloned folder
- Select `project.json` to load the project

### 3. Restore Dependencies

UiPath Studio will automatically resolve and install the required NuGet packages listed in `project.json`. If prompted, click **Yes** to restore dependencies.

### 4. Configure Orchestrator Asset

- Navigate to your **Orchestrator** instance (or local folder assets)
- Create the `Sender_Gmail` credential asset as described in [Prerequisites](#orchestrator-asset-setup)

### 5. Run the Bot

- Click ▶️ **Run** in UiPath Studio (or press `F5`)
- Enter your desired **search topic** when prompted
- Enter the **recipient email address** when prompted
- Sit back and let the bot deliver the news! 📬

---

## 📂 Project Structure

```
News-Generator/
│
├── Main.xaml               # Primary workflow — entry point of the automation
├── project.json            # UiPath project configuration and dependency manifest
├── project.uiproj          # UiPath project metadata
├── entry-points.json       # Defines the workflow entry points
├── README.md               # Project documentation (this file)
├── .gitignore              # Git ignore rules
│
├── .entities/              # UiPath entity definitions
├── .local/                 # Local development settings
├── .objects/               # UiPath object repository
├── .project/               # Internal project metadata
├── .screenshots/           # UI element screenshots for selectors
├── .settings/              # Project-level settings
├── .storage/               # Local storage files
├── .templates/             # Workflow templates
└── .tmh/                   # UiPath modern design metadata
```

---

## 🔧 Configuration

### Email Settings

The bot uses **Gmail SMTP** with the following default configuration:

| Setting | Value |
|---------|-------|
| SMTP Server | `smtp.gmail.com` |
| Port | `587` |
| Enable SSL | `True` |
| Authentication | Via Orchestrator `Sender_Gmail` asset |

### Customization Options

| What to Change | Where to Change | How |
|----------------|-----------------|-----|
| Default browser | `Main.xaml` | Replace Edge activities with Chrome/Firefox equivalents |
| Email provider | `Main.xaml` | Update SMTP server and port settings |
| Number of articles | `Main.xaml` | Modify the data extraction range |
| Freshness filter | `Main.xaml` | Change the timestamp keyword from `"hours"` to `"minutes"`, etc. |

---

## 🐛 Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| **Browser doesn't open** | Edge not installed or outdated | Install/update Microsoft Edge to the latest version |
| **No news results found** | Topic too niche or Google layout changed | Try a broader search term; verify Google News page structure |
| **Email not sent** | Invalid credentials or App Password not set | Regenerate your Gmail App Password and update the Orchestrator asset |
| **"Selector not found" error** | Google UI elements changed | Update the selectors in `Main.xaml` using UiPath's **Indicate on Screen** feature |
| **Orchestrator asset error** | `Sender_Gmail` asset missing | Create the credential asset in Orchestrator (see [Prerequisites](#orchestrator-asset-setup)) |
| **SMTP authentication failed** | 2-Step Verification not enabled | Enable 2-Step Verification on your Google Account before generating an App Password |

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve this project:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m "Add amazing feature"`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Ideas for Contribution

- 🌍 Multi-language news support
- 📊 Extract and email multiple articles (top 5/10)
- 📅 Schedule the bot to run daily via Orchestrator triggers
- 🗂️ Save extracted news to an Excel or CSV file
- 🔔 Add Slack or Teams notification support

---

## 📬 Contact

If you have any questions, suggestions, or feedback — feel free to reach out!

- **GitHub:** [your-username](https://github.com/your-username)

---

<p align="center">
  Made with ❤️ using <strong>UiPath Studio</strong>
</p>
]]>
