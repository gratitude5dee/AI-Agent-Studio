## Project info

**URL**: https://lovable.dev/projects/ef9ce659-f174-403e-b9b6-ff35eaf32e75

# Project Launch Pad: AI Agent Studio

<div align="center">
  <img src="https://raw.githubusercontent.com/user-attachments/assets/da9d81d2-b676-47b7-873b-e3cde4a74288" alt="Project Launch Pad Logo" width="150">
  <h1>Project Launch Pad</h1>
  <p><strong>A full-stack platform for creating, managing, and deploying multimodal AI agents.</strong></p>
  <p>Build everything from simple chatbots to complex, voice-enabled "Digital Twins" with a secure, serverless-first architecture.</p>
</div>

<p align="center">
  <img src="https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React">
  <img src="https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white" alt="Supabase">
  <img src="https://img.shields.io/badge/Vite-B73BFE?style=for-the-badge&logo=vite&logoColor=FFD62E" alt="Vite">
  <img src="https://img.shields.io/badge/pipecat-EF5A25?style=for-the-badge&logo=python&logoColor=white" alt="Pipecat AI">
</p>

**Project Launch Pad** provides the foundation for building sophisticated AI agent applications. It features a React-based frontend for creating and managing AI "Digital Twins" and a powerful backend system that combines Supabase with a separate Python server for real-time voice and text chat. This starter kit is perfect for developers looking to build next-generation AI-native products.

## ✨ Features

-   **🤖 AI Agent Creator:** A guided wizard to create custom AI agents ("Digital Twins") with unique personalities, instructions, and capabilities.
-   **🗣️ Real-time Voice & Text Chat:** Engage with your agents through a low-latency, multimodal chat interface powered by **Pipecat**.
-   **🔒 Secure Serverless Backend:** Leverages **Supabase** for database, auth, and storage, plus a suite of **Deno Edge Functions** that act as a secure proxy to AI services.
-   **🔌 Pluggable AI Models:** Integrates with **Fal.ai** for generative tasks (like image creation), with all API keys securely managed on the backend.
-   **🎛️ Rich UI & Components:** Built with **shadcn/ui** and **Tailwind CSS**, providing a beautiful and highly customizable user interface.
-   **⚡ Modern & Fast:** Powered by **Vite** with **SWC** for a lightning-fast development experience.
-   **🔐 Protected Routes & Authentication:** Full user authentication flow managed by Supabase Auth.

## 🏛️ Architecture

This project uses a dual-backend architecture to handle both standard web requests and real-time voice communication efficiently.

```
┌──────────────────┐    ┌───────────────────────────┐      ┌────────────────┐
│  React Frontend  ├─►  │   Supabase Edge Functions   ├─────►│  3rd Party APIs  │
│ (Vite, shadcn)   │    │  (Deno, Secure API Proxy)   │      │ (Fal.ai, etc.) │
└──────────────────┘    └───────────────────────────┘      └────────────────┘
        │                            ▲
        │                            │ (Database, Auth, Storage)
        │ (WebSockets/WebRTC)        │
        ▼                            │
┌──────────────────┐    ┌─────────────────────┐
│ Python Backend   ├─►  │  Supabase Platform  │
│ (Pipecat, Gemini)│    └─────────────────────┘
└──────────────────┘
```

1.  **Frontend:** The React client handles the UI and user interactions.
2.  **Supabase:** Manages user data, authentication, project metadata, and serverless functions.
3.  **Supabase Edge Functions:** Act as a secure middle layer. They receive requests from the frontend, securely attach API keys (e.g., for Fal.ai), and forward them to the appropriate services.
4.  **Python Backend:** A dedicated server using the Pipecat framework handles the real-time, low-latency WebSocket and WebRTC connections required for voice conversations.

## ✅ Prerequisites

Before you begin, ensure you have the following installed and configured:

-   [Node.js](https://nodejs.org/) (v18.0.0 or higher)
-   A package manager: `npm`, `yarn`, or `bun`
-   [Python](https://www.python.org/downloads/) (v3.10, 3.11, or 3.12)
-   [Supabase CLI](https://supabase.com/docs/guides/cli)
-   API Keys:
    -   A [Supabase account](https://supabase.com/dashboard)
    -   A [Fal.ai account](https://fal.ai/) and API Key
    -   A [Google AI Studio API Key](https://aistudio.google.com/app/apikey) for Gemini
    -   (Optional) A [Daily.co API Key](https://dashboard.daily.co/signup) for WebRTC voice mode

## 🚀 Quick Start: Full System Setup

Getting this project running involves setting up the Supabase backend, the Python voice server, and the frontend.

### Part 1: Supabase Backend Setup

1.  **Clone the Frontend Repository:**
    ```bash
    git clone https://github.com/your-username/project-launch-pad-core-91-main.git
    cd project-launch-pad-core-91-main
    ```

2.  **Link to Your Supabase Project:**
    -   Log in to the Supabase CLI: `supabase login`
    -   Create a new project on the [Supabase Dashboard](https://supabase.com/dashboard).
    -   Link the local project: `supabase link --project-ref <YOUR_PROJECT_ID>`
      *(You can find `<YOUR_PROJECT_ID>` in your project's URL)*

3.  **Configure Environment Variables:**
    -   Create a file named `.env.local` in the project root.
    -   Find your **Project URL** and **`anon` public key** in your Supabase project's API settings and add them to the file.

    **.env.local**
    ```env
    VITE_SUPABASE_URL="YOUR_SUPABASE_PROJECT_URL"
    VITE_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"
    VITE_SERVER_URL="http://127.0.0.1:7860/api"
    ```

4.  **Set Supabase Secrets:**
    Set your Fal.ai API key as a secret. The edge functions will use this to make secure requests.
    ```bash
    supabase secrets set FAL_KEY="YOUR_FAL_API_KEY"
    ```
    *(Your Fal key should be in the format `key_id:key_secret`)*

5.  **Apply Database Migrations:**
    Push the local database schema from `supabase/migrations` to your remote Supabase project.
    ```bash
    supabase db push
    ```

6.  **Deploy Edge Functions:**
    ```bash
    supabase functions deploy --no-verify-jwt
    ```

### Part 2: Python Voice Server Setup

This project requires a separate Python server for real-time voice capabilities.

1.  **Clone the Backend Repository:**
    ```bash
    git clone https://github.com/pipecat-ai/gemini-multimodal-live-demo.git
    cd gemini-multimodal-live-demo/server
    ```

2.  **Set up the Python Environment:**
    ```bash
    python3 -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    pip install -r requirements.txt
    ```

3.  **Configure Backend Environment Variables:**
    -   Copy the example environment file: `cp env.example .env`
    -   Edit the newly created `.env` file and add your **Google Gemini API Key** and your optional **Daily API Key**.

4.  **Run the Python Server:**
    ```bash
    python sesame.py run
    ```
    The server will start, by default on `http://127.0.0.1:7860`.

### Part 3: Frontend Setup

1.  **Navigate back to the frontend project directory:**
    ```bash
    cd ../../project-launch-pad-core-91-main
    ```

2.  **Install dependencies and run the app:**
    ```bash
    npm install
    npm run dev
    ```

Your application should now be running on `http://localhost:8080`, fully connected to both backends!

## 🤝 Contributing

We enthusiastically welcome contributions! Whether you're fixing a bug, adding a new feature, or improving documentation, your help is invaluable.

1.  **Fork** the repository.
2.  **Create a new branch:** `git checkout -b feature/your-new-feature`.
3.  **Make your changes** and commit them with clear, descriptive messages.
4.  **Push** your branch to your forked repository.
5.  **Open a Pull Request** to the `main` branch.

### Areas for Contribution

-   **Testing:** Implement a testing suite using Vitest and React Testing Library.
-   **New Agent Tools:** Add new capabilities or integrations for the AI agents.
-   **UX/UI Enhancements:** Improve the user experience in the agent creation or chat interfaces.
-   **Documentation:** Enhance documentation for components, hooks, and services.

## 📜 License

This project is open-source. Please add a `LICENSE` file to the repository to specify the terms (e.g., MIT, Apache 2.0).
