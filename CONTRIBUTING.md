<p align="center">
  <!-- A placeholder for your future logo -->
  <!-- <img src="URL_TO_YOUR_LOGO.svg" alt="DialogWeaver Logo" width="150"> -->
</p>

<h1 align="center">DialogWeaver</h1>

<p align="center">
  <strong>The open-source framework for weaving intelligent, real-time voice conversations.</strong>
</p>

<p align="center">
  <a href="https://github.com/Hiteshydv001/DialogWeaver/blob/main/LICENSE"><img src="https://img.shields.io/github/license/Hiteshydv001/DialogWeaver?style=flat-square" alt="License: MIT"></a>
  <a href="https://github.com/Hiteshydv001/DialogWeaver/issues"><img src="https://img.shields.io/github/issues/Hiteshydv001/DialogWeaver?style=flat-square" alt="GitHub issues"></a>
  <a href="https://github.com/Hiteshydv001/DialogWeaver/blob/main/CONTRIBUTING.md"><img src="https://img.shields.io/badge/PRs-Welcome-brightgreen?style=flat-square" alt="PRs Welcome"></a>
  <a href="#"><img src="https://img.shields.io/static/v1?label=Discord&message=Join%20Chat&color=7289DA&logo=discord&style=flat-square" alt="Join the community on Discord"></a>
</p>

---

**DialogWeaver** is a complete, self-hostable framework designed to create, manage, and deploy sophisticated, real-time voice assistants. It provides a high-performance orchestration engine, a secure multi-tenant API, and a no-code UI playground—all fully open-source.

With DialogWeaver, you can seamlessly **weave** together best-in-class AI services (Speech-to-Text, LLMs, and Text-to-Speech) to build fluid, interruptible, and intelligent conversational agents for telephony, web applications, and more.

## Table of Contents

-   [Core Features](#core-features)
-   [Architecture Explained](#architecture-explained)
-   [Local Development Setup](#local-development-setup)
-   [How to Test the Platform](#how-to-test-the-platform)
-   [API Overview & Endpoints](#api-overview--endpoints)
-   [How to Contribute](#how-to-contribute)
-   [License](#license)

## Core Features

-   **🎙️ Real-Time & Interruptible:** Natural, human-like conversation logic that supports low-latency streaming and allows users to interrupt the agent.
-   **🧩 Extensible & Provider-Agnostic:** Easily integrate with any ASR, LLM, or TTS provider to find the perfect balance of cost and performance.
-   **🔐 Secure API & UI:** A FastAPI backend and Next.js UI for secure, multi-user agent management with encrypted key storage.
-   **📞 Telephony & Web Ready:** Natively supports both traditional phone calls (via Twilio/Plivo) and real-time web-based clients.
-   **🚀 Self-Hostable with Docker:** Deploy the entire platform on your own infrastructure with a single command for complete data privacy and control.
-   **🎨 No-Code Agent Builder:** A user-friendly interface to visually construct, test, and manage your voice agents.

## Architecture Explained

DialogWeaver is built with a clean, service-oriented architecture. Think of it as having three main parts that work together:

1.  **The Control Panel (`api` service):**
    *   This is the secure **FastAPI** backend that manages everything: user accounts, agent configurations, and all the secret API keys (which it encrypts in the database). When you want to make a call, you talk to this service.

2.  **The Conversation Engine (`engine` service):**
    *   This is a specialized **Python worker** built for one job: handling the live conversation. It processes the audio stream in real-time, talks to the AI services (for transcription, thinking, and speaking), and handles interruptions.

3.  **The Dashboard (`ui` service):**
    *   This is the **Next.js** web application you see in your browser. It's a user-friendly dashboard where you can build agents with forms and buttons instead of code. It only talks to the `api` service.

These three services, along with a **PostgreSQL** database and a **Redis** cache, run together in **Docker** containers, making the whole system self-contained and easy to run anywhere.

---

## Local Development Setup

Follow these steps to get the complete DialogWeaver platform running on your local machine.

### 1. Prerequisites

-   **Git:** To clone the repository.
-   **Docker & Docker Compose:** The easiest way to get both is to [Install Docker Desktop](https://www.docker.com/products/docker-desktop/).
-   **An ngrok Account:** Required to receive webhooks from telephony providers. [Sign up for free](https://ngrok.com/).

### 2. Clone the Repository

```bash
git clone https://github.com/Hiteshydv001/DialogWeaver.git
cd DialogWeaver
```

### 3. Configure Your Environment

This is the most important step. You need to provide API keys for the services you want to use.

-   First, create your local environment file from the sample:
    ```bash
    cp .env.sample .env
    ```
-   Now, open the newly created `.env` file in your code editor and fill in the required values.
    -   **Crucially, generate a new `SECRET_KEY`**. You can run `openssl rand -hex 32` in your terminal to get one.
    -   Fill in your `NGROK_AUTHTOKEN`.
    -   Fill in the credentials for at least one provider for each category (Telephony, ASR, LLM, TTS), for example: `TWILIO_*`, `DEEPGRAM_AUTH_TOKEN`, `OPENAI_API_KEY`, and `ELEVENLABS_API_KEY`.

### 4. Run the Platform

With your `.env` file configured, start all services using Docker Compose.
```bash
docker compose up --build
```
> **Tip:** Run this command *without* the `-d` flag the first time. This will show you the logs from all services directly in your terminal, which is essential for debugging.

### 5. Verify the Setup

Once the containers are running, check that each component is accessible:

-   **Frontend UI**: Open `http://localhost:3000`
-   **Backend API Docs**: Open `http://localhost:8000/docs`
-   **Ngrok Dashboard**: Open `http://localhost:4040` to get your public URL (e.g., `https://random-string.ngrok-free.app`). **Copy this public URL.**

---

## How to Test the Platform

### 1. Configure Telephony Webhook

-   Go to your Twilio (or Plivo) account's phone number settings.
-   Under "Voice & Fax", in the "A CALL COMES IN" section, set the webhook to your **ngrok public URL** from the previous step, pointing to the correct endpoint: `https://<your-ngrok-url>/twilio`.
-   Set the HTTP method to `POST` and save.

### 2. Use the DialogWeaver UI

1.  **Register:** Go to `http://localhost:3000` and create a new user account.
2.  **Add Keys:** Navigate to the "API Keys" page and add your API keys for OpenAI, ElevenLabs, etc. The backend will encrypt and store them.
3.  **Create an Agent:** Go to the "Agents" page, click "Create New Agent", and use the form to configure its personality and tools.
4.  **Make a Call:** Click the "Make a Call" button, enter **your personal phone number**, and start a conversation!

---

## API Overview & Endpoints

You can interact with the API directly at `http://localhost:8000/docs`. Here are the key endpoints:

#### User Authentication

-   `POST /api/v1/users/register`: Create a new user account.
-   `POST /api/v1/users/token`: Log in to get a JWT access token.

**Example: Get a token**
```bash
# First, set your token to an environment variable for convenience
export TOKEN=$(curl -X POST "http://localhost:8000/api/v1/users/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=your-email@example.com&password=your-password" | jq -r .access_token)
```

#### Agent Management

-   `POST /api/v1/agents/`: Create a new agent.
-   `GET /api/v1/agents/`: List all of your agents.

**Example: Create an agent**
```bash
# Create a file named agent.json with your agent's configuration
curl -X POST "http://localhost:8000/api/v1/agents/" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d @agent.json
```

#### Initiating Calls

-   `POST /api/v1/calls/{agent_id}/initiate_call`: Start a call using a specific agent.

**Example: Make a call**
```bash
curl -X POST "http://localhost:8000/api/v1/calls/1/initiate_call" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"phone_number": "+12223334444"}'
```

---

## How to Contribute

We welcome contributions of all kinds! This is how you can help.

### Reporting Bugs & Suggesting Features

-   **Found a bug?** [Open a Bug Report](https://github.com/Hiteshydv001/DialogWeaver/issues/new?template=bug_report.md).
-   **Have an idea?** [Suggest a new Feature](https://github.com/Hiteshydv001/DialogWeaver/issues/new?template=feature_request.md).

### Your First Contribution (The 5-Minute PR)

A great way to start is by improving documentation.
1.  Find a typo or a sentence that could be clearer in this `README.md` or any other documentation file.
2.  Click the "Edit" (pencil) icon on the file in the GitHub UI.
3.  Make your change and write a clear commit message (e.g., `docs: Fix typo in setup guide`).
4.  Click "Propose changes" and open a pull request. That's it!

### Submitting a Pull Request

1.  **Fork** the repository and clone your fork.
2.  **Create a branch** for your changes: `git checkout -b feat/add-new-provider`.
3.  **Make your changes** locally.
4.  **Commit your work** with a descriptive message: `git commit -m "feat(tts): Add support for a new TTS provider"`.
5.  **Push** to your fork: `git push origin feat/add-new-provider`.
6.  **Open a Pull Request** from your fork to the `main` branch of `Hiteshydv001/DialogWeaver`.
7.  In your PR description, link to the issue it resolves (e.g., `Closes #42`).

## License

DialogWeaver is released under the **[MIT License](LICENSE)**.
```
