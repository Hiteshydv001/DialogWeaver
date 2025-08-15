<p align="center">
  <img src="https://github.com/Hiteshydv001/DialogWeaver/blob/main/docs/logo.jpg" alt="DialogWeaver Logo" width="400">
</p>

<h1 align="center">DialogWeaver</h1>

<p align="center">
  <strong>Weave complex, real-time voice AI conversations. The end-to-end, open-source orchestration platform for building intelligent voice agents.</strong>
</p>

<p align="center">
  <a href="https://github.com/Hiteshydv001/DialogWeaver/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT"></a>
  <a href="https://github.com/Hiteshydv001/DialogWeaver/issues"><img src="https://img.shields.io/github/issues/your-username/dialogweaver" alt="GitHub issues"></a>
  <a href="https://github.com/Hiteshydv001/DialogWeaver/blob/main/docs/Contribution.md"><img src="https://img.shields.io/badge/PRs-Welcome-brightgreen.svg" alt="PRs Welcome"></a>
  <a href="#"><img src="https://img.shields.io/static/v1?label=Discord&message=Join%20Chat&color=7289DA&logo=discord" alt="Join the community on Discord"></a>
</p>

---

**DialogWeaver** is a complete, self-hostable framework designed to create, manage, and deploy sophisticated, real-time voice-first conversational assistants. It provides all the necessary components as open-source, giving you full control and ownership over your voice AI applications.

Whether you're a developer prototyping a new voice-driven feature, a business building a customer service agent, or a researcher experimenting with conversational AI, DialogWeaver provides the production-grade foundation you need.

## Key Features

*   **🎙️ Real-Time & Interruptible:** A high-performance core that orchestrates complex voice conversations, allowing agents to be interrupted mid-sentence for a natural and fluid user experience.
*   **🧩 Extensible & Provider-Agnostic:** Seamlessly integrates with best-in-class ASR (Speech-to-Text), LLM (Large Language Model), and TTS (Text-to-Speech) providers. Easily add new providers to suit your needs.
*   **🔐 Secure & Multi-Tenant API:** A robust FastAPI backend handles user authentication, agent management, and the secure, encrypted storage of provider API keys, making it ready for multi-user environments.
*   **🎨 No-Code UI Playground:** A modern Next.js web interface provides a visual playground to build, test, and manage your voice agents without writing a single line of code.
*   **📞 Telephony & Web Ready:** Natively supports both traditional telephony (via providers like Twilio and Plivo) and web-based clients through a unified, containerized architecture.
*   **🚀 Dockerized & Self-Hostable:** The entire platform is orchestrated with Docker Compose, allowing you to deploy it on your own infrastructure with a single command, ensuring data privacy and control.

## Architecture

DialogWeaver is built as a modern, service-oriented monorepo. This design provides a clean separation of concerns and enhances scalability and maintainability.

| Service       | Description                                                                 | Key Technologies                                   |
|---------------|-----------------------------------------------------------------------------|----------------------------------------------------|
| **`ui`**      | User-facing dashboard and no-code agent builder.                            | `Next.js`, `React`, `TypeScript`, `Tailwind CSS`   |
| **`api`**     | Secure RESTful control plane for managing users, agents, and calls.           | `FastAPI`, `Python`, `PostgreSQL`, `SQLAlchemy`    |
| **`engine`**  | The real-time worker that orchestrates the voice conversation logic.        | `FastAPI (as worker)`, `Python`, `WebSockets`      |
| **`database`**| Persistent relational data storage.                                         | `PostgreSQL`                                       |
| **`cache`**   | In-memory data store for caching and session management.                    | `Redis`                                            |

![DialogWeaver Architecture Diagram](https://github.com/Hiteshydv001/DialogWeaver/blob/main/docs/Architecture.png)


---

## Quick Start

Get the entire DialogWeaver platform running on your local machine in just a few steps.

**Prerequisites:**
*   [Docker](https://www.docker.com/products/docker-desktop/) and Docker Compose
*   API keys from your chosen AI and telephony providers (see `.env.sample`)

### 1. Clone the Repository
```bash
git clone https://github.com/OpenVoiceX/DialogWeaver.git
cd DialogWeaver
```

### 2. Configure Your Environment
Create your local environment file from the template. This file will store your secret keys and is ignored by Git.
```bash
cp .env.sample .env
```
Now, open the `.env` file and fill in your API keys and credentials. A `SECRET_KEY` and `NGROK_AUTHTOKEN` are required for the initial setup.

### 3. Build and Run the Platform
This single command builds all the service images and starts the containers.
```bash
docker compose up --build
```
> For the first run, omit the `-d` flag to see the logs from all services in your terminal. This is the best way to debug any setup issues.

### 4. Initialize the Database
Once the containers are running, open a **new terminal window** and run the database migrations to set up your tables.
```bash
docker compose exec api alembic revision --autogenerate -m "Initial schema"
docker compose exec api alembic upgrade head
```

### 5. Access Your Platform
Your DialogWeaver instance is now live!
-   **UI Playground**: [http://localhost:3000](http://localhost:3000)
-   **API Documentation**: [http://localhost:8000/docs](http://localhost:8000/docs)
-   **Ngrok Dashboard** (for your public telephony URL): [http://localhost:4040](http://localhost:4040)

To learn how to configure your telephony provider and make your first call, please see our detailed [Contributor Guide](CONTRIBUTING.md).

---

## Contributing

DialogWeaver is a community-driven project. We welcome contributions of all kinds, from bug reports to new features and documentation improvements.

Please read our **[CONTRIBUTING.md](CONTRIBUTING.md)** to get started with the development workflow and code style.

## License

This project is licensed under the MIT License - see the **[LICENSE](LICENSE)** file for details.



