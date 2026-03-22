# 🤖 CyberGuard AI Chatbot

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python)
![NVIDIA](https://img.shields.io/badge/NVIDIA-NIM_API-76B900?style=for-the-badge&logo=nvidia)
![Llama4](https://img.shields.io/badge/Meta-Llama_4_Maverick_17B-0064e0?style=for-the-badge&logo=meta)
![Colab](https://img.shields.io/badge/Google-Colab_Ready-F9AB00?style=for-the-badge&logo=googlecolab)
![Free](https://img.shields.io/badge/Cost-100%25_FREE-brightgreen?style=for-the-badge)
![Conversations](https://img.shields.io/badge/Memory-Multi--Turn_Conversations-purple?style=for-the-badge)

> **An AI-powered cybersecurity chatbot that remembers your full conversation.**
> Ask anything about cybersecurity — powered by NVIDIA NIM API + Meta Llama 4.
> Runs 100% free in your browser via Google Colab. No GPU. No installation.

---

## 🎯 What Makes This Special

Most AI demos just answer **one question** and forget everything.

CyberGuard is different:

| Feature | Basic API Call | CyberGuard Chatbot |
|---|---|---|
| Answers questions | ✅ | ✅ |
| Remembers previous messages | ❌ | ✅ |
| Has a personality/role | ❌ | ✅ (Cybersecurity Expert) |
| Multi-turn conversation | ❌ | ✅ |
| Runs in browser | ✅ | ✅ |

This is how **real production chatbots** work — using conversation history to maintain context.

---

## 💬 What You Can Ask CyberGuard

```
🛡️  "What is spear phishing?"
🔐  "How do I create a strong password?"
🚨  "What should I do if I clicked a phishing link?"
🏢  "How do companies protect against cyber attacks?"
📧  "Is this email safe?" (paste any email)
🔍  "What is social engineering?"
💻  "How do hackers break into systems?"
🛡️  "What is two-factor authentication?"
```

---

## 🏗️ How It Works — Architecture

```
You type a message
        ↓
Added to conversation_history[]
        ↓
Full history sent to NVIDIA NIM API
        ↓
Meta Llama 4 (17B parameters) processes it
        ↓
Response added back to history
        ↓
Bot replies — remembering everything said before
```

This is called a **stateful conversation loop** — the same pattern used by ChatGPT, Claude, and all major AI assistants.

---

## 🚀 Quick Start — 3 Steps

### Step 1 — Get Free NVIDIA API Key
1. Go to [build.nvidia.com](https://build.nvidia.com)
2. Click **"Get API Key"** (green button)
3. Sign in with your NVIDIA/Google account
4. Copy your key: `nvapi-xxxxxxxxxxxxxxxxx`

### Step 2 — Open Google Colab
1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click **"New Notebook"**

### Step 3 — Run the Chatbot
Paste the code, add your API key, press **Shift + Enter**

---

## ⚙️ Setup in Colab

### Cell 1 — Install Library

![Setup Screenshot](screenshot_setup.png)

```python
!pip install requests
```

> Requirements already satisfied confirms the library is ready instantly in Google Colab ✅

---

## 💻 Full Chatbot Code

![Chatbot Code](screenshot_chatbot_code.png)

```python
import requests

invoke_url = "https://integrate.api.nvidia.com/v1/chat/completions"

headers = {
    "Authorization": "Bearer YOUR_NVIDIA_API_KEY",  # Get free at build.nvidia.com
    "Accept": "application/json"
}

# Stores the entire conversation — this is what gives the bot MEMORY
conversation_history = [
    {
        "role": "system",
        "content": """You are CyberGuard, an expert AI cybersecurity assistant.
        You help users understand:
        - Phishing and spear phishing attacks
        - How to stay safe online
        - How to detect suspicious emails
        - Cybersecurity best practices
        Be friendly, clear, and simple. Use emojis to make it engaging."""
    }
]

print("=" * 50)
print("🛡️  CyberGuard — Powered by NVIDIA NIM + Llama 4")
print("=" * 50)
print("Ask me anything about cybersecurity!")
print("Type 'quit' to exit\n")

while True:
    user_input = input("You: ")

    if user_input.lower() in ["quit", "exit", "bye"]:
        print("CyberGuard: Stay safe online! Goodbye! 🛡️")
        break

    if not user_input.strip():
        continue

    # Add user message to history
    conversation_history.append({
        "role": "user",
        "content": user_input
    })

    payload = {
        "model": "meta/llama-4-maverick-17b-128e-instruct",
        "messages": conversation_history,  # Full history = memory!
        "max_tokens": 1024,
        "temperature": 0.7,
        "stream": False
    }

    response = requests.post(invoke_url, headers=headers, json=payload)
    bot_reply = response.json()["choices"][0]["message"]["content"]

    # Save reply to history so next message has full context
    conversation_history.append({
        "role": "assistant",
        "content": bot_reply
    })

    print(f"\nCyberGuard: {bot_reply}\n")
    print("-" * 50)
```

---

## 📊 Real Output — AI Answering Cybersecurity Questions

![Chatbot Output](screenshot_chatbot_output.png)

> **Asked:** "What is phishing?"
>
> **CyberGuard replied with:**
> - A clear definition of phishing
> - Types: Login credentials, financial info, personal data
> - Attack channels: Emails, SMS (smishing), Phone (vishing), Social media
> - Common tactics: Spoofing, Urgency, Social engineering
>
> All explained in plain English, in under 3 seconds, using a 17-billion parameter AI model — completely free.

---

## 🧠 The Key Concept — Conversation Memory

The secret behind how this chatbot remembers everything:

```python
# Every message gets added to history
conversation_history.append({"role": "user", "content": user_input})

# The FULL history is sent every time — not just the latest message
payload = {
    "messages": conversation_history,  # ← This is the magic!
}

# Bot reply also saved back
conversation_history.append({"role": "assistant", "content": bot_reply})
```

**This is exactly how ChatGPT works internally.** 🔥

---

## 🧪 Example Conversation

```
════════════════════════════════════════════
🛡️  CyberGuard — Powered by NVIDIA NIM + Llama 4
════════════════════════════════════════════
Ask me anything about cybersecurity!
Type 'quit' to exit

You: What is spear phishing?

CyberGuard: Spear phishing is a targeted form of phishing where
attackers customize their attack for a specific person. Unlike
generic phishing, spear phishing uses your name, company, or
role to seem more legitimate. It's the #1 cause of enterprise
data breaches. 🎯

──────────────────────────────────────────────

You: How do I protect myself from it?

CyberGuard: Great follow-up! Based on what we just discussed
about spear phishing, here are your best defenses:
1. ✅ Always verify sender email addresses carefully
2. ✅ Never click links — go directly to the website
3. ✅ Enable two-factor authentication everywhere
4. ✅ When in doubt, call the sender directly

──────────────────────────────────────────────

You: quit

CyberGuard: Stay safe online! Goodbye! 🛡️
```

---

## 🧰 Tech Stack

| Component | Technology | Cost |
|---|---|---|
| AI Model | Meta Llama 4 Maverick 17B | 🆓 Free |
| API Endpoint | NVIDIA NIM API | 🆓 Free |
| HTTP Requests | Python `requests` | 🆓 Free |
| Notebook | Google Colab | 🆓 Free |
| Memory System | Conversation history array | 🆓 Free |

**Total cost: $0.00** ✅

---

## 📂 Project Structure

```
📦 cyberguard-ai-chatbot/
 ┣ 📓 cyberguard_chatbot.ipynb       ← Run this in Google Colab!
 ┣ 📓 general_ai_chatbot.ipynb       ← General purpose chatbot
 ┣ 📄 README.md                      ← You are here
 ┣ 📄 requirements.txt               ← requests==2.31.0
 ┣ 🖼️ screenshot_setup.png           ← Library installation
 ┣ 🖼️ screenshot_chatbot_code.png    ← Code in Colab
 ┗ 🖼️ screenshot_chatbot_output.png  ← AI response output
```

---

## 🔮 Future Roadmap

- [ ] 🌐 Gradio web UI — chatbot in a proper browser interface
- [ ] 💾 Save conversation history to file
- [ ] 📊 Threat severity scoring for each answer
- [ ] 🔗 Connect to phishing detector (Part 1 project)
- [ ] 📱 WhatsApp/Telegram bot integration
- [ ] 🗣️ Voice input/output support

---

## 🔗 Related Project

This chatbot is part of a larger AI cybersecurity suite:

👉 **[AI Spear Phishing Detector](https://github.com/MohammadZakir939/ai-cybersecurity-threat-detection)**
> Analyzes emails and detects phishing with risk scores and red flags

---

## 🎯 Key Learnings

- How **conversation memory** works in LLM chatbots
- The **system/user/assistant** role structure in chat APIs
- **Stateful conversation loops** in Python
- **Prompt engineering** with role-based personas
- Calling **NVIDIA NIM API** for production-grade AI inference
- Running **17B parameter models** free via cloud APIs

---

## ⚙️ Requirements

```
requests==2.31.0
```

No other dependencies needed! Everything else runs on Google Colab's built-in environment.

---

## 🙏 References

- [NVIDIA NIM API Documentation](https://build.nvidia.com)
- [Meta Llama 4 Model](https://ai.meta.com/llama/)
- [NVIDIA Morpheus Cybersecurity Framework](https://developer.nvidia.com/morpheus-cybersecurity)

---

## 👨‍💻 About

Built as part of an AI cybersecurity portfolio.
Started with zero Python installed — built entirely in a browser tab.

**⭐ Star this repo if it helped you!**

---

*Built with ❤️ using NVIDIA NIM API + Meta Llama 4 Maverick 17B*
