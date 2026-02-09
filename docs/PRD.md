This is a comprehensive **Product Requirements Document (PRD)** for **Revive**.

You can feed this document directly into **Antigravity** (or any AI coding agent) context to ensure it understands not just the *code*, but the *business logic* and *user flow* perfectly.

---

# Product Requirements Document (PRD): Revive

| **Project Name** | Revive (The AI Reactivation Engine) |
| --- | --- |
| **Version** | 1.0 (MVP) |
| **Status** | **Development** |
| **Owner** | Ashwin (Founder) |
| **Core Objective** | To automate the reactivation of "dead" leads for high-ticket local businesses (Jewelry, Real Estate) using AI-driven WhatsApp conversations and interactive tools. |

---

## 1. Executive Summary

**Revive** is a B2B SaaS platform that allows businesses to upload lists of past/dormant leads and deploy autonomous AI agents to re-engage them via WhatsApp. Unlike standard chatbots, Revive focuses on **outcome-based workflows** (Bookings, Sales).

**Key Differentiators:**

1. **Interactive "Tap-to-Act" Flows:** Uses WhatsApp Buttons instead of text-only reliance.
2. **Visual Selling:** Integrated Generative AI for "Virtual Try-On" experiences.
3. **Safety First:** Includes a Simulator and Throttling Engine to prevent hallucinations and bans.

---

## 2. User Personas

1. **The Admin (You/Agency Owner):** Needs granular control over prompts, API keys, and system health.
2. **The Client (Mr. Rokde / Builder):** Non-technical. Wants to see "Money In" (Appointments). Needs a simple dashboard and daily WhatsApp reports.
3. **The End User (The Lead):** Needs a frictionless, non-spammy experience. Prefers tapping buttons over typing text.

---

## 3. Functional Requirements (The "What")

### 3.1 Authentication & Onboarding

* **Requirement:** Multi-tenant login system.
* **Methods:**
* `Admin Login`: Google OAuth (for superuser access).
* `Client Login`: Email/Password (for business owners).


* **UI:** Minimalist, enterprise-grade white card design.

### 3.2 The Dashboard (Command Center)

* **Top Bar:** Displays Client Name and **Credit Balance** (Real-time decrement).
* **Key Metrics (3D Cards):**
* `Total Leads`: Count of uploaded rows.
* `Active Conversations`: Leads currently in the 24-hour window.
* `Appointments Booked`: Count of successful tool triggers.


* **Primary Action:** Prominent "+ New Campaign" button.

### 3.3 Campaign Creation Wizard

**Step 1: Data Injection (Import)**

* **Input:** Drag-and-drop CSV/XLSX.
* **Smart Mapping Engine:**
* System must parse headers and suggest maps:
* `Phone` / `Mobile` -> `target_phone`
* `Name` / `Customer` -> `first_name`
* `City` / `Product` -> `context_variable`




* **Validation:** Regex check for phone numbers. Auto-fix button to prepend `+91`.

**Step 2: Agent Architect (Config)**

* **Persona Config:** Inputs for Name (e.g., "Sneha"), Role, and Tone Slider (0-100).
* **Tool Toggles:**
* **Interactive Buttons (Crucial):** UI to define button labels (e.g., "Try On 📸", "Check Price 💰").
* **Image Generation:** Upload reference folder. Trigger logic tied to specific Button Payload ID.
* **Calendar:** Link input for Calendly/Google Calendar.
* **Price Negotiation:** Toggle ON/OFF (Hard rule in System Prompt).



**Step 3: Safety Simulator**

* **Requirement:** Sandbox chat environment using the configured System Prompt.
* **Feature:** Dropdown to select a specific row from the CSV to test variable injection.
* **Functionality:** Must render WhatsApp Buttons in the simulator and support clicking them to trigger the bot's response.

**Step 4: The Launchpad**

* **Throttling Engine:** Backend constraint setting speed to **10 messages/minute**.
* **Confirmation:** Summary modal showing Estimated Cost and Target Count.

### 3.4 The Live Monitor (Kanban)

* **Real-time Board:** Columns must update via WebSockets.
* `Sent` -> `Delivered` -> `Read` -> `Button Clicked` -> `Responded` -> `HOT LEAD` (High Intent).


* **Human Handover:** Clicking a card opens a full chat modal. Admin can type a message to override the AI.

### 3.5 Automated Logic ("Secret Sauce")

* **Morning Briefing:** Cron job runs at 09:00 IST. Aggregates stats from previous 24h and sends a WhatsApp summary to the Client User.
* **Red Flag Alert:** Sentiment analysis on incoming messages. If Sentiment = `Negative` OR Keywords = `["stop", "spam", "police"]`:
* Immediately mark lead status as `DND`.
* Kill AI loop for that `lead_id`.
* Notify Admin.


* **Re-Engagement Loop:** If sentiment is `Neutral/Postpone` (e.g., "Ask me next month"), schedule a Celery task for `T + 30 days`.

---

## 4. Technical Architecture

### 4.1 Tech Stack

* **Frontend:** Next.js 14 (React), Tailwind CSS, Framer Motion (Animations).
* **Backend:** Python FastAPI (Async).
* **Database:** Supabase (PostgreSQL).
* **Task Queue:** Celery + Redis (Dockerized).
* **AI Engine:** OpenAI `gpt-4o` (Main Brain) + `dall-e-3` or `stable-diffusion` (Image Gen).
* **Messaging:** WhatsApp Cloud API (Meta).

### 4.2 Database Schema (Core Tables)

* **`clients`**: `id`, `name`, `whatsapp_phone_id`, `api_key`.
* **`campaigns`**: `id`, `client_id`, `status`, `prompt_config`.
* **`leads`**: `id`, `campaign_id`, `phone`, `context_data (JSON)`, `status`, `sentiment_score`.
* **`messages`**: `id`, `lead_id`, `role`, `content`, `type (text/interactive/image)`.

### 4.3 API Endpoints (Core)

* `POST /api/v1/webhook`: Verifies Meta token, handles `messages` and `statuses` updates.
* `POST /api/v1/campaigns/upload`: Parses CSV, creates Lead rows.
* `POST /api/v1/simulation/chat`: Stateless endpoint for the Test Simulator.
* `GET /api/v1/monitor/stats`: Returns real-time counts for the dashboard.

---

## 5. UI/UX Guidelines

* **Philosophy:** "Futuristic Enterprise."
* **Animations:**
* **Parallax:** Dashboard cards tilt on hover.
* **Transitions:** Page transitions should use "wipe" or "fade-up" effects.
* **Loading:** Use skeleton loaders for the Kanban board.


* **Feedback:**
* **Success:** Confetti for "Campaign Launched".
* **Error:** Shake animation on invalid inputs.



---

## 6. Success Metrics (KPIs)

1. **System Stability:** 99.9% Uptime on Webhook Listener.
2. **Safety:** < 0.1% Hallucination rate (AI promising wrong prices).
3. **Efficiency:** Rate limiting holds strict at 10 msgs/min (Zero WhatsApp Bans).
4. **Engagement:** > 15% Button Click-through Rate (CTR) on "Try On" / "Book Visit".

---

## 7. Roadmap

* **Phase 1 (MVP - 2 Weeks):** Core Dashboard, CSV Upload, Text + Button Sending, Basic Throttling.
* **Phase 2 (Polish):** Image Generation Tool, Morning Briefing, Red Flag Alert.
* **Phase 3 (Scale):** Multi-agent orchestration (different agents for different stages of the funnel).