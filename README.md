# NextEdge: AI-Powered CRM Automation

![NextEdge Logo](frontend/public/Logo.png)

NextEdge is an innovative platform designed to streamline CRM processes through intelligent AI automation. It seamlessly integrates a modern Vite + React + TypeScript frontend with a robust FastAPI backend, offering powerful features for managing customer interactions, automating data entry, and enhancing overall productivity.

## ✨ Features

*   **AI-Powered Gmail Sync**: Automatically pull unread Gmail messages and attachments, process them with Gemini 2.0 Flash, and generate structured CRM upsert plans.
*   **Intelligent Data Validation**: Ensure data integrity with AI-driven validation of structured responses before CRM integration.
*   **Seamless Zoho CRM Integration**: Connect effortlessly with Zoho CRM to execute AI-generated plans, update records, and manage customer relationships.
*   **Secure Authentication**: Leverages Clerk for secure user authentication and management.
*   **Scalable Architecture**: Built with FastAPI for high performance and a Vite/React frontend for a dynamic user experience.
*   **Developer-Friendly Setup**: Easy to configure and run locally for development and testing.

## 🚀 Getting Started

Follow these steps to get NextEdge up and running on your local machine.

### Prerequisites

Ensure you have the following installed:

*   Node.js (LTS version recommended)
*   Python 3.9+
*   Git

### 1. Clone the Repository

```bash
git clone https://github.com/kowsik11/NextEdge_Dev.git
cd NextEdge_Dev
```

### 2. Configuration

Environment files for Clerk and integration settings are crucial for the application to function correctly.

*   `frontend/.env`: Contains the Clerk publishable key and frontend API domain used by Vite.
*   `backend/.env`: Stores Clerk credentials, Google OAuth values, Gemini key, HubSpot token, and the frontend URL.

**Example `.env` files:**

`frontend/.env`
```
VITE_CLERK_PUBLISHABLE_KEY=pk_YOUR_CLERK_PUBLISHABLE_KEY
VITE_API_DOMAIN=http://localhost:8000
```

`backend/.env`
```
CLERK_SECRET_KEY=sk_YOUR_CLERK_SECRET_KEY
GOOGLE_CLIENT_ID=YOUR_GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET=YOUR_GOOGLE_CLIENT_SECRET
GEMINI_API_KEY=YOUR_GEMINI_API_KEY
HUBSPOT_API_KEY=YOUR_HUBSPOT_API_KEY
FRONTEND_URL=http://localhost:8081
```

### 3. Frontend Setup

Navigate to the `frontend` directory, install dependencies, and start the development server.

```powershell
cd frontend
npm install
npm run dev
```

Open the URL printed in the terminal (default `http://localhost:8081`) in your browser.

### 4. Backend Setup

Navigate to the `backend` directory, set up a virtual environment, install dependencies, and start the FastAPI server.

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

The backend will run on `http://localhost:8000`.

## 💡 Usage

### Gmail Sync Workflow

1.  **Launch Frontend and Backend**: Ensure both the frontend (`npm run dev`) and backend (`uvicorn app.main:app --reload --port 8000`) are running.
2.  **Connect Google**: Visit `http://localhost:8081/home` and click **Connect Google** to complete the OAuth process.
    *   **Important**: In Google Cloud Console, add both `http://localhost:8000/api/google/callback` **and** `http://127.0.0.1:8000/api/google/callback` as authorized redirect URIs. Use whichever host you prefer in `backend/.env`.
3.  **Trigger Pipeline**: Trigger the pipeline with a `POST` request to `http://127.0.0.1:8000/api/pipeline/run` supplying JSON such as:

    ```json
    {
      "user_id": "<clerk_user_id>",
      "max_messages": 3,
      "execute_zoho": true
    }
    ```
    The backend will:
    *   Pull unread Gmail messages and attachments.
    *   Send consolidated text to Gemini 2.0 Flash.
    *   Validate the structured response and produce a CRM upsert plan.
    *   Optionally execute the plan against Zoho CRM when `execute_zoho` is `true`.
    *   You can also run a one-off cycle from the command line with `python -m app.cli`.

### Zoho CRM Connection

1.  **Connect Zoho**: From `/home`, click **Connect Zoho** (requires being logged in through Clerk). The page redirects to Zoho OAuth and, after consent, returns to `/home?connected=zoho`.
2.  **Status Display**: Connection status and the Zoho email are displayed on the home page.
3.  **Token Management**: Tokens are stored per Clerk user in `zoho_tokens.json`, with automatic refresh handled server-side.

## 📂 Project Structure

*   `frontend/`: Vite React application. All UI code resides in `frontend/src`.
*   `backend/`: FastAPI backend, services, and integration logic.
*   `infra/`, `scripts/`: Reserved for deployment and automation tooling.

## 🤝 Contributing

We welcome contributions to NextEdge! Please see our `CONTRIBUTING.md` (coming soon) for guidelines on how to submit pull requests, report bugs, and suggest new features.

## 📄 License

This project is licensed under the MIT License - see the `LICENSE` file (coming soon) for details.

## 📞 Support

For any questions or support, please open an issue on the GitHub repository.
