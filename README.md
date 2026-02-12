
<div align="center">
  <br />
  <a href="https://github.com/revive-ai/revive">
    <img src="https://ui-avatars.com/api/?name=Revive+AI&background=6366f1&color=fff&size=128&rounded=true&font-size=0.33" alt="Logo" width="100" height="100">
  </a>

  <h1 align="center">💎 Revive AI</h1>

  <p align="center">
    <b>The AI Operating System for Modern Jewelry Commerce</b>
    <br />
    <a href="https://revive.ai/demo">View Demo</a>
    ·
    <a href="https://github.com/revive-ai/revive/issues">Report Bug</a>
    ·
    <a href="https://github.com/revive-ai/revive/issues">Request Feature</a>
  </p>
</div>

<div align="center">
    <img src="https://img.shields.io/badge/Status-Alpha-orange?style=for-the-badge" alt="Status" />
    <img src="https://img.shields.io/badge/Version-1.0.0-blue?style=for-the-badge" alt="Version" />
    <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License" />
</div>

<br />

> **"Turn dormant leads into loyal customers with intelligent, automated conversations."**

---

## 🌟 Introduction

**Revive AI** is not just a chatbot. It is a full-stack **CRM & Marketing Automation Engine** built specifically for high-value retail (Jewelry, Luxury Goods). It combines the power of **Large Language Models (LLMs)** with the ubiquity of **WhatsApp** to create a seamless, human-like sales experience.

Imagine a sales associate who:
*   Never sleeps 🌙
*   Remembers every customer's preference 🧠
*   Shows the right product at the right time 💍
*   Books appointments instantly 📅

That is **Revive**.

---

## 🚀 Key Features

### 🧠 Intelligent "Brain"
*   **Context-Aware**: Remembers conversation history (last 5 messages) to maintain natural flow.
*   **Dynamic Persona**: Configurable "Agents" (e.g., *Riya*, *Sarah*) with adjustable Tone, Emoji use, and Sales Aggression.
*   **RAG Knowledge Base**: Upload PDFs/CSVs of your inventory, and the AI "knows" what you have in stock.

### 💬 WhatsApp Native Commerce
*   **Interactive Shopping**: Send Product Carousels, Catalogs, and "Add to Cart" buttons directly in chat.
*   **Deep Linking**: One-tap actions to "Book Appointment" or "Claim Discount".
*   **Smart Flows**: Native forms for collecting customer details without leaving WhatsApp.

### 🔬 Command Center
*   **Live Matrix**: Real-time log of every thought process, API call, and message sent.
*   **Fleet Management**: Visual dashboard to manage multiple "Agents" running simultaneously.
*   **CRM**: Bulk CSV Import, Auto-Tagging, and Smart Segmentation (e.g., "VIP > $10k").

### 🛡️ Enterprise Safety
*   **Throttling**: Intelligent rate limiting (e.g., 10 msgs/min) to prevent bans.
*   **Sentiment Guard**: Automatically detects anger/frustration and escalates to human staff.
*   **Red Flag System**: instantly blocks abusive users.

---

## 🏗️ Architecture

Revive is built on a modern, scalable stack designed for reliability and speed.

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend** | `FastAPI` (Python) | High-performance async API handling Webhooks & Logic. |
| **Brain** | `Mistral AI` | The LLM powering the conversation (via `mistral-tiny`). |
| **Database** | `Supabase` (Postgres) | Storing Leads, Messages, Vector Embeddings (pgvector). |
| **Queue** | `Celery` + `Redis` | Handling background tasks (Blasts, Reports). |
| **Frontend** | `Next.js 14` | The "Command Center" dashboard for business owners. |
| **Channel** | `WhatsApp Business API` | The primary communication interface. |

---

## 🛠️ Installation (Local Dev)

Follow these steps to get Revive running on your machine.

### Prerequisites
*   Python 3.10+
*   Node.js 18+
*   Supabase Account
*   Meta Developer Account (WhatsApp API)
*   Redis (Local or Cloud)

### 1. Clone the Repository
```bash
git clone https://github.com/revive-ai/revive.git
cd revive
```

### 2. Backend Setup
```bash
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env  # Fill in your keys!
python3 -m app.main
```

### 3. Frontend Setup
```bash
cd frontend
npm install
cp .env.local.example .env.local # Fill in Supabase Keys
npm run dev
```

### 4. Expose Localhost (Webhook)
```bash
ngrok http 8000
# Copy the URL (https://xyz.ngrok-free.app) to Meta Developer Portal
```

---

## 📸 Screenshots

<div align="center">
  <img src="https://via.placeholder.com/800x400?text=Command+Center+Dashboard" alt="Dashboard" />
  <p><i>The "Command Center" - Real-time metrics & pipeline view.</i></p>
</div>

<br />

<div align="center">
  <img src="https://via.placeholder.com/400x700?text=WhatsApp+Conversation" alt="Chat" width="300" />
  <p><i>The "Agent" in action - Natural, helpful, and sales-driven.</i></p>
</div>

---

## 🗺️ Roadmap

- [x] **Phase 1**: Core Engine & Database (Completed)
- [x] **Phase 2**: AI Integration (Mistral) (Completed)
- [x] **Phase 3**: Basic Dashboard (Next.js) (Completed)
- [x] **Phase 6**: Re-Engagement & Verification (Completed)
- [ ] **Phase 7**: Cloud Deployment (In Progress)
- [ ] **Phase 8**: RAG Knowledge Base
- [ ] **Phase 9**: Native Commerce (Catalogs)

---

## 🤝 Contributing

We welcome contributions! Please see `CONTRIBUTING.md` for details.

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

<div align="center">
  <p>Built with ❤️ by the Revive Team</p>
</div>
