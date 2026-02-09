# Phases of Development: Project "Revive"

**Goal:** Build a production-ready AI Reactivation Engine for WhatsApp (FastAPI + Next.js + Supabase).
**Total Estimated Timeline:** 4-6 Weeks (Aggressive Sprint).

---

## Phase 1: The Foundation (Infrastructure & Data Layer)
**Objective:** Set up the "spine" of the application. By the end of this phase, we should have a running server, a database, and a verified connection to WhatsApp.

### 1.1 Project Scaffolding
- [ ] Initialize Git repository (Monorepo or separate `frontend`/`backend`).
- [ ] **Backend:** Set up FastAPI structure (`app/main.py`, `app/core/config.py`, `app/api`).
- [ ] **Frontend:** Initialize Next.js 14 project with Tailwind CSS and Shadcn UI.
- [ ] **Environment:** Create `.env` template handling `SUPABASE_URL`, `OPENAI_API_KEY`, `WHATSAPP_TOKEN`, and `WEBHOOK_VERIFY_TOKEN`.
- [ ] Configure Docker & Docker Compose (for local Redis/Celery).

### 1.2 Database Architecture (Supabase)
- [ ] Create Supabase Project.
- [ ] Run SQL Migrations for Core Tables:
    - `clients` (Auth, WhatsApp ID, API Keys)
    - `campaigns` (Configuration, Template Name, Status)
    - `leads` (The Data Rows with JSONB context, Status, Last Interaction)
    - `messages` (Chat History: User vs Assistant)
- [ ] Set up Row Level Security (RLS) policies.

### 1.3 WhatsApp Connectivity ("The Handshake")
- [ ] Set up Meta Developer App & WhatsApp Business Account.
- [ ] Implement `GET /webhook` endpoint in FastAPI to handle the Meta Challenge verification (`hub.verify_token`).
- [ ] Implement basic `POST /webhook` logging to verify we receive incoming messages.

**Definition of Done:**
- Server is running locally via `uvicorn`.
- Database tables exist and are connected.
- Meta Developer Portal shows "Webhook Verified" (Green Checkmark).

---

## Phase 2: The Core Engine (Templates & Basic AI)
**Objective:** Enable the system to ingest data and initiate the *legal* WhatsApp conversation flow.

### 2.1 The "Data Injection" Module
- [ ] **Backend:** Build `POST /campaigns/upload` endpoint.
    - Implement `pandas` logic to read CSV/XLSX.
    - Implement "Smart Mapping" logic (Auto-detect `phone`, `name`).
    - Validate phone numbers (Regex `^\+?[1-9]\d{1,14}$`) and sanitize (auto-append `+91`).
    - Bulk Insert into `leads` table.
- [ ] **Frontend:** Build the "Drag & Drop" UI using `react-dropzone`.
    - Display the "Preview Table" with mapping dropdowns.

### 2.2 The "Template Gateway" (CRITICAL STEP)
*Note: WhatsApp requires the first message to be a pre-approved Template.*
- [ ] **Backend:** Create `WhatsAppService.send_template(phone, template_name, variables)`.
- [ ] **Logic:** The "Blast" triggers this function, NOT the AI.
- [ ] **Frontend:** Add input to select/enter the "Template Name" (e.g., `diwali_offer_v1`) during Campaign Config.

### 2.3 The "Brain" (LLM Integration)
- [ ] Set up `OpenAIService` (or Anthropic).
- [ ] Create `ConversationManager` class:
    - **Logic:** Listen for webhook events. IF `messages.type == text` OR `button_reply`, THEN trigger AI.
    - Fetch last 5 messages from `messages` table.
    - Construct the System Prompt dynamically based on Client Config (e.g., "You are Sneha...").
- [ ] Implement the Basic Chat Loop:
    - User Replies to Template -> Webhook -> Store -> AI Generates -> Store -> Send Free-form Text.

**Definition of Done:**
- You can upload a CSV.
- You receive a **Template Message** on your phone.
- When you reply to that template, the **AI responds** with context.

---

## Phase 3: The "Interactive Upgrade" (Buttons & Tools)
**Objective:** Move beyond text. Implement the "Tap-to-Act" features that drive conversion.

### 3.1 Interactive Message Handling
- [ ] **Backend:** Create `WhatsAppMessageBuilder` utility.
    - Function to construct `interactive` payloads (Quick Reply Buttons, List Messages, Call-to-Action).
- [ ] **Webhook Handling:** Update `POST /webhook` to handle `type: interactive`.
    - Extract `list_reply.id` and `button_reply.id` payload (e.g., `try_on_trigger`).

### 3.2 The "Toolbelt" Implementation
- [ ] **Image Generation Tool:**
    - Build `generate_image(prompt, reference_img_url)` function.
    - Integrate DALL-E 3 or Stable Diffusion API.
    - **Trigger Logic:** *If Payload ID == 'try_on_trigger', Bypass LLM -> Call Image Gen -> Send Media Message.*
- [ ] **Calendar Tool:**
    - Create logic to inject a "Book Now" link/button when intent is classified as "High/Ready".

### 3.3 The "Campaign Config" UI
- [ ] Build the "Agent Architect" screen in Next.js.
- [ ] Add Toggles for "Interactive Buttons" and "Image Gen".
- [ ] Store button labels and tool configurations in `campaigns.config` JSONB column.

**Definition of Done:**
- The Bot sends a message with "Try On" and "Check Price" buttons.
- Clicking "Try On" triggers an image response instantly without the AI getting confused.

---

## Phase 4: Safety, Simulator & Throttling (The Guardrails)
**Objective:** Make the system safe for business use and scalable.

### 4.1 The Throttling Engine (The Leaky Bucket)
- [ ] Set up **Celery** with **Redis**.
- [ ] Implement the `send_campaign_task` worker.
    - Use `rate_limit='10/m'` (Strict 10 tasks per minute).
    - Logic: Check `leads.status`. If `new`, send Template. If `DND`, skip.
- [ ] **Frontend:** Add the "Speedometer" visualization to the Launchpad.

### 4.2 The Safety Simulator
- [ ] **Backend:** Create `POST /simulate` endpoint (Stateless).
    - Inputs: `message`, `row_data` (mock lead), `prompt_config`.
    - Returns: AI response text/media *without* calling WhatsApp API.
- [ ] **Frontend:** Build the iPhone Mockup component.
    - Allow users to "Chat" with their configured agent in the browser.

### 4.3 Red Flag System
- [ ] Implement Sentiment Analysis (Lightweight GPT-Mini check or VADER).
- [ ] **Safety Logic:**
    - IF sentiment == `Negative` OR keyword in `['stop', 'spam', 'report']`:
    - THEN update `leads.status = 'DND'` AND `leads.blocked = true`.
    - Stop all future Celery tasks for this number.

**Definition of Done:**
- Launching a campaign of 50 leads takes exactly 5 minutes (throttled).
- The Simulator works perfectly.
- Typing "STOP" immediately kills the bot for that user.

---

## Phase 5: The "Command Center" (Frontend Polish)
**Objective:** Build the beautiful dashboard the client sees.

### 5.1 Dashboard & Stats
- [ ] Implement `GET /stats` endpoint (Aggregation queries).
- [ ] Build 3D Tilt Cards for "Total Leads", "Active", "Booked".
- [ ] Implement the **Kanban Board**.
    - Columns: Sent, Delivered, Read, Replied, Hot Lead.
    - Enable Drag-and-Drop (optional manual override).

### 5.2 Real-time Updates
- [ ] Implement **Supabase Realtime** (WebSockets) on the `messages` table.
    - When a new row is inserted, the Kanban card should "pop" or move automatically on the frontend.

### 5.3 Authentication & Multi-Tenancy
- [ ] Finalize Login Screens.
- [ ] Audit RLS Policies: Ensure Client A cannot access Client B's leads.

**Definition of Done:**
- The Dashboard looks like the "Futuristic Glassmorphism" design.
- Sending a message from your phone makes a card move on the dashboard instantly.

---

## Phase 6: The "Secret Sauce" & Deployment
**Objective:** Production readiness and retention features.

### 6.1 The "Morning Briefing"
- [ ] Set up `Celery Beat` (Cron Scheduler).
- [ ] Task: Runs at 09:00 Client Local Time.
    - SQL Query: Count interactions & bookings in last 24h.
    - Format WhatsApp Template (Admin Report).
    - Send to Client Admin's phone number.

### 6.2 The Re-Engagement Loop
- [ ] Logic: If AI detects "Not now" or "Next month", insert into `scheduled_tasks` table for `NOW() + 30 Days`.

### 6.3 Deployment
- [ ] **Database:** Production Supabase instance.
- [ ] **Backend:** Deploy Docker container to Railway/Render/AWS ECS.
- [ ] **Frontend:** Deploy to Vercel/Netlify.
- [ ] **Domain:** Connect `app.revive.ai` (or similar).

**Definition of Done:**
- The system runs autonomously.
- Clients receive their morning reports.
- You are ready to onboard Rokde Jewellers.