# J.A.R.V.I.S
# 🧠 Jarvis AI Assistant Setup Guide

Welcome to the Jarvis AI Assistant — a voice-based AI system that uses:

- 🤖 **n8n** for tool execution and backend routing  
- 🗣️ **Eleven Labs** for voice-based conversational AI  
- 💻 **Lovable.dev** for the modern front-end web interface

This guide walks you through setting up each component in the correct order.

---

## 📦 Repository Contents

This repo includes:
This repo includes:

• [n8n/](n8n/) — JSON files for all necessary workflows (main agent and child tools)  
• [Eleven Labs Setup.md](Eleven%20Labs%20Setup.md) — Full instructions to configure the voice agent  
• [n8n setup.md](n8n%20setup.md) — Full backend integration steps  
• [Lovable Setup.md](Lovable%20Setup.md) — Frontend integration process using Lovable.dev  

---

## 🚀 Step 1: Set Up `n8n` (The Brain)

> n8n is the backend workflow orchestrator responsible for executing actions like checking your calendar, sending emails, and more.

### ✅ Steps:

1. **Import Workflows**
   - Download all JSON files in the `n8n/` directory.
   -  n8n → click "Import from file" → import each JSON workflow.

2. **Connect Agents**
   - The main workflow (`J.A.R.V.I.S Main Agent`) routes requests to child workflows:
     - `Calendar Agent`
     - `Email Agent`
     - `Contact Agent`
     - `Research Agent`
     

3. **Insert Credentials**
   - Provide your:
     - Gemini API Key
     - Tavily API Key
     - Airtable credentials (for Contact Agent)

4. **Configure Webhook Trigger**
   - Each workflow starts with a webhook (`POST` method).
   - Copy the **test webhook URL** for now.

5. **Respond to Webhook**
   - Ensure the final node in each workflow is a **“Respond to Webhook”** block, configured to send the final response after tool execution completes.

6. **Enable Workflows**
   - Activate all workflows when you're ready for production.
   - Switch Eleven Labs URLs from test to production accordingly.

---

## 🎙️ Step 2: Set Up Eleven Labs (The Voice)

> Eleven Labs hosts the conversational AI personality — modeled after **J.A.R.V.I.S.** — and connects it to your n8n backend.

### ✅ Steps:

1. **Create an Agent**
   - Go to the **Conversational AI → My Agents**
   - Create a new agent and name it `Jarvis`

2. **Configure Personality & System Prompt**
   - Use the prompt provided in `Eleven Labs Setup.md`  
   - Jarvis has a dry, sarcastic tone and always refers to the user as "sir"

3. **Add a Tool**
   - Add a `Custom Tool` named `n8n`
   - Set the method to `POST`
   - URL = the webhook URL from n8n
   - Add a body parameter:
     - `query` (type: string) — maps to the user's spoken request

4. **Select a Voice**
   - Choose or create a British male voice using the provided description in the setup file

5. **Get the Widget Code**
   - Go to **Widget settings** and copy the embed code
   - This will be used in the next step

---

## 🌐 Step 3: Set Up Lovable.dev (The Interface)

> Lovable.dev builds your futuristic, voice-driven UI that users will interact with.

### ✅ Steps:

1. **Go to [Lovable.dev](https://lovable.dev)**  
2. **Describe Your App**
   - Use this prompt:
     ```
     I want to build a simple and modern web interface to interact with a voice agent using ElevenLabs...
     ```

3. **Embed Eleven Labs Widget**
   - Paste the `<elevenlabs-convai>` embed code you copied earlier
   - Ask Lovable to:
     - Center the widget
     - Replace orb with an image (e.g., Jarvis icon)
     - Change “Start to call” to “Talk to Jarvis”
     - Set compact mode and apply dark/techy theme

4. **Customize the Interface**
   - Use Tailwind CSS or React (optional)
   - Ask for features like:
     - Mic button
     - Chat-style message view
     - Pulsing animations or futuristic elements

---

## 🧪 Final Testing

- Speak to Jarvis from the frontend
- Ensure Eleven Labs receives the audio, transcribes it, and sends it to n8n
- Check that n8n returns a response and it displays properly
- Activate workflows to go live!

---

## 🛠 Troubleshooting Tips

- If you see errors in Eleven Labs, check:
  - Is the webhook active in n8n?
  - Did you switch from test URL to production?
  - Are credentials (Gemini/Tavily) added?
- Add logging nodes in n8n for debugging.
- Be explicit in the system prompt about when to use tools.

---

## 💡 Credits

- Voice Agent: [Eleven Labs](https://elevenlabs.io)
- Workflow Automation: [n8n](https://n8n.io)
- UI Builder: [Lovable.dev](https://lovable.dev)

Built with sarcasm, precision, and your questionable productivity in mind.  
_“Now let’s act like you know what you're doing, sir.”_

📺 [Watch this reference video on YouTube](https://www.youtube.com/watch?v=KUvSzvFeZls&t=473s)

---
