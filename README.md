# WhatsApp Google Drive Automation with n8n

## Objective
This project creates an **n8n workflow** that listens to WhatsApp messages and performs various Google Drive operations such as listing files, deleting, moving, and summarizing documents in specified folders.

---

## Features / Functional Requirements

### 1. Inbound WhatsApp Channel
- Uses **Twilio Sandbox for WhatsApp** (or any WhatsApp Cloud API) as the entry point to receive messages.
- Parses simple text commands from WhatsApp messages:
  - `LIST /ProjectX` — Lists files in the `/ProjectX` folder.
  - `DELETE /ProjectX/report.pdf` — Deletes the specified file.
  - `MOVE /ProjectX/report.pdf /Archive` — Moves a file from `/ProjectX` to `/Archive`.
  - `SUMMARY /ProjectX` — Returns a bullet-point digest summary of each file’s content (supports PDF, Docx, TXT).

### 2. Google Drive Integration
- Uses **OAuth2 authentication** to securely access only the authenticated user’s Google Drive.
- Utilizes MIME-type aware nodes in n8n to properly read and process different file types.

### 3. AI Summarization
- Instead of OpenAI, this project uses the **DeepSeek Gemma model** for generating concise summaries of documents within n8n.

### 4. Logging and Safety
- Maintains an audit spreadsheet/log of all operations for traceability.
- Includes safeguards against accidental mass deletion, such as requiring a confirmation keyword.

### 5. Deployment
- Includes a ready-to-import `workflow.json` for n8n.
- Instructions to run n8n with Docker for easy deployment.

---

## Getting Started

### Prerequisites
- Docker and Docker Compose installed on your system.
- A Google Cloud project with OAuth2 credentials (Client ID & Client Secret) for Google Drive API access.
- A Twilio account with WhatsApp Sandbox enabled.
- DeepSeek Gemma API access credentials.
- if using local server than use ngrok for making your local server link with a free sample public server ip so twilio can make requests (only for local server using)

### Setup Instructions

#### 1. Twilio WhatsApp Sandbox
- Log in to your Twilio console.
- Navigate to **Messaging > Try it Out > WhatsApp Sandbox**.
- Follow the instructions to join the sandbox by sending the join code from your WhatsApp.
- Configure the webhook URL in Twilio to point to your n8n webhook endpoint (e.g., `https://your-n8n-domain.com/webhook/whatsapp-gdrive`).

#### 2. Google Drive OAuth2 Setup
- Create OAuth2 credentials on Google Cloud Console for Desktop or Web application.
- Set the authorized redirect URI to your n8n instance URL plus `/rest/oauth2-credential/callback`.
- Download the Client ID and Client Secret.
- Import these credentials into n8n's Google Drive OAuth2 credentials.

#### 3. DeepSeek Gemma Model
- Obtain API credentials for DeepSeek Gemma.
- Configure n8n to use the DeepSeek Gemma API endpoint and your credentials in the AI summarization node.

#### 4. Running n8n with Docker
- Clone this repo:
  ```bash
  git clone https://github.com/harshkh-001/whatsapp_drive_automation.git
  cd whatsapp_drive_automation
  ```
- For running:
  ```bash
  docker-compose up -d
  ```
  - Make sure your docker is running during run command

#### 5. Ngrok Setup
  - login to ngrok and optain authtoken
  - Then in local terminal type :
    ```bash
    ngrok config add-authtoken $YOUR_AUTHTOKEN
    ngrok http $your_port
    ```
  - By Default port which n8n uses is 5678 

## 📜 License

This project is licensed under the MIT License — see the LICENSE file.
