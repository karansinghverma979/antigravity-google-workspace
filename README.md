<div align="center">

# 🌐 Antigravity Google Workspace MCP Server

### *Unified, High-Speed Model Context Protocol (MCP) Server for the Entire Google Workspace Ecosystem*

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![MCP Protocol](https://img.shields.io/badge/MCP-1.0.0-8A2BE2?style=for-the-badge&logo=anthropic&logoColor=white)](https://modelcontextprotocol.io)
[![Google Cloud](https://img.shields.io/badge/Google_Cloud-APIs-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white)](https://cloud.google.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)
[![Zero Secret Leak](https://img.shields.io/badge/Security-Zero_Secret_Exposure-brightgreen?style=for-the-badge)](vibe-security)

<p align="center">
  <b>Seamlessly empower AI agents with autonomous read/write access to Calendar, Tasks, Gmail, Drive, Docs, Sheets, and Contacts.</b>
</p>

---

</div>

## 📑 Table of Contents
- [✨ Features](#-features)
- [🏛️ System Architecture](#️-system-architecture)
- [🛠️ Tool Catalog (22 Operations)](#️-tool-catalog-22-operations)
- [🚀 Quickstart & Setup](#-quickstart--setup)
  - [1. Prerequisites](#1-prerequisites)
  - [2. Google Cloud OAuth Setup](#2-google-cloud-oauth-setup)
  - [3. Installation & Authentication](#3-installation--authentication)
- [⚙️ Client Configurations](#️-client-configurations)
  - [Antigravity CLI](#antigravity-cli)
  - [Claude Desktop](#claude-desktop)
  - [Cursor / Windsurf / Other MCP Hosts](#cursor--windsurf--other-mcp-hosts)
- [🔒 Security & Token Hygiene](#-security--token-hygiene)
- [📄 License](#-license)

---

## ✨ Features

- ⚡ **All-In-One Unified Server**: A single lightweight Python MCP server providing access to 7 core Google Workspace services without juggling multiple server instances.
- 📬 **Full Gmail Operations**: List messages, fetch full thread content, generate drafts, send emails, and trash spam.
- 📅 **Bi-directional Calendar Sync**: List upcoming events, create new schedules with attendee invites, and cancel events.
- 📝 **Google Tasks Integration**: Manage task lists, create tactical tasks with due dates, and mark completions.
- 📂 **Google Drive & Docs Intelligence**: Query files, read docs, create new Google Docs, and append content dynamically.
- 📊 **Spreadsheets Engine**: Read tabular data, append rows, and update spreadsheet cell ranges.
- 📇 **Google Contacts Management**: Search contact lists and create new address book entries.
- 🔐 **Zero-Leak Architecture**: Local OAuth token caching with automated token refresh and strict gitignore isolation.

---

## 🏛️ System Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                      AI CLIENT / HOST AGENT                      │
│            (Antigravity CLI / Claude Desktop / Cursor)            │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ JSON-RPC (stdio)
                                  ▼
┌──────────────────────────────────────────────────────────────────┐
│             ANTIGRAVITY GOOGLE WORKSPACE MCP SERVER              │
│                       (server.py / FastMCP)                      │
├──────────────────────────────────────────────────────────────────┤
│ 🔐 OAuth Token Engine (Auto-Refresh & Local Token Storage)       │
├──────────────────────────────────────────────────────────────────┤
│ 📅 Calendar  │ 📬 Gmail     │ 📂 Drive & Docs │ 📇 Contacts       │
│ 📝 Tasks     │ 📊 Sheets    │ 🔒 Auth Guard   │ ⚡ Error Filter   │
└─────────────────────────────────┬────────────────────────────────┘
                                  │ HTTPS (Google APIs v1/v3/v4)
                                  ▼
┌──────────────────────────────────────────────────────────────────┐
│                 GOOGLE WORKSPACE CLOUD PLATFORM                  │
│       [ Gmail · Calendar · Tasks · Drive · Docs · Sheets ]       │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tool Catalog (22 Operations)

| Service | MCP Tool Name | Description |
| :--- | :--- | :--- |
| **📅 Calendar** | `gcal_list_events` | Fetch upcoming events with start/end limits |
| | `gcal_create_event` | Schedule meetings with summary, times, and attendees |
| | `gcal_delete_event` | Remove or cancel an existing calendar event |
| **📝 Tasks** | `gtasks_list_tasks` | List active tasks from specified task list |
| | `gtasks_create_task` | Create new task with title, notes, and due date |
| | `gtasks_complete_task` | Mark a specific task item as completed |
| | `gtasks_delete_task` | Delete a task from Google Tasks |
| **📬 Gmail** | `gmail_list_messages` | Search emails with queries (`from:`, `is:unread`, etc.) |
| | `gmail_get_message` | Retrieve full headers, subject, and body of an email |
| | `gmail_create_draft` | Prepare draft email without sending immediately |
| | `gmail_send_message` | Dispatch emails to recipients with CC/BCC support |
| | `gmail_trash_message` | Move a message directly to trash |
| **📂 Drive** | `gdrive_search_files` | Search files by name, MIME type, or full-text query |
| | `gdrive_read_file` | Read plain text / markdown content of Drive files |
| | `gdrive_trash_file` | Move files to Google Drive trash |
| **📝 Docs** | `gdocs_create_doc` | Create a new Google Document with initial title |
| | `gdocs_read_doc` | Extract structured body text from a Google Doc |
| | `gdocs_append_text` | Append text paragraphs or markdown content into Doc |
| **📊 Sheets** | `gsheets_read_range` | Read 2D row/column data from a spreadsheet range |
| | `gsheets_append_row` | Append one or more rows to an active sheet |
| | `gsheets_update_range`| Overwrite or update specific matrix cell ranges |
| **📇 Contacts** | `gcontacts_list` | List names, emails, and phone numbers in Contacts |
| | `gcontacts_create` | Add new contact entry to Google Contacts |

---

## 🚀 Quickstart & Setup

### 1. Prerequisites
- Python 3.10+ installed
- A Google Account with Google Cloud Platform access

### 2. Google Cloud OAuth Setup

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project (e.g. `Antigravity-Workspace`).
3. Enable the following **7 APIs** under **APIs & Services > Library**:
   - Google Calendar API
   - Tasks API
   - Gmail API
   - Google Drive API
   - Google Docs API
   - Google Sheets API
   - People API (Google Contacts)
4. Configure the **OAuth Consent Screen**:
   - User Type: **External** (or Internal for Workspace organizations).
   - Add your Google account as a **Test User**.
5. Create OAuth Credentials:
   - Navigate to **Credentials > Create Credentials > OAuth Client ID**.
   - Application Type: **Desktop App**.
   - Name: `Antigravity-Workspace-Client`.
6. Download the generated client configuration JSON and save it as `credentials.json` in the root of this project:
   ```bash
   cp ~/Downloads/client_secret_*.json ./credentials.json
   ```

### 3. Installation & Authentication

```bash
# 1. Clone the repository
git clone https://github.com/your-username/antigravity-google-workspace.git
cd antigravity-google-workspace

# 2. Install dependencies
pip install -r requirements.txt

# 3. Authenticate with Google
python auth_setup.py
```
*A local browser window will open at `http://localhost:8088`. Sign in, grant permissions, and `token.json` will be saved locally.*

---

## ⚙️ Client Configurations

### Antigravity CLI
Add to your `mcp_config.json` or Antigravity MCP settings:

```json
{
  "mcpServers": {
    "google-workspace": {
      "command": "python",
      "args": [
        "C:\\Users\\<USER>\\.gemini\\google-workspace\\server.py"
      ]
    }
  }
}
```

### Claude Desktop
Add to `%APPDATA%\Claude\claude_desktop_config.json` (Windows) or `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

```json
{
  "mcpServers": {
    "google-workspace": {
      "command": "python",
      "args": [
        "/path/to/antigravity-google-workspace/server.py"
      ]
    }
  }
}
```

---

## 🔒 Security & Token Hygiene

- **Zero Hardcoded Secrets**: Client secrets and live tokens are strictly isolated in `credentials.json` and `token.json`.
- **Git Ignored**: `.gitignore` strictly blocks all tokens, secrets, `.env`, and session states from ever entering source control.
- **Local Token Refresh**: Expired tokens are refreshed automatically on the fly without storing plaintext credentials in memory.

---

## 🧠 Official Companion Skill & Autonomous Governance

This MCP server is natively governed and orchestrated by the **[`gsuite`](https://github.com/karansinghverma979/antigravity-custom-skills/blob/main/gsuite/SKILL.md)** skill from the **[`antigravity-custom-skills`](https://github.com/karansinghverma979/antigravity-custom-skills)** suite.

- 📖 **Skill Specification**: [`gsuite/SKILL.md`](https://github.com/karansinghverma979/antigravity-custom-skills/blob/main/gsuite/SKILL.md)
- 🌐 **Master Skillpack Suite**: [Antigravity Custom Skills](https://github.com/karansinghverma979/antigravity-custom-skills)

---

## 📄 License

This project is open-source and licensed under the [MIT License](LICENSE).

