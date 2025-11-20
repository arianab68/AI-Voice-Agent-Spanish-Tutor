# Carmen - AI Spanish Voice Tutor

An agentic AI voice assistant that teaches Spanish through natural conversation. Built to explore how AI agents can autonomously adapt their teaching approach based on user interactions.

---

## The Product

Carmen uses agentic AI to decide in real-time whether to teach vocabulary, practice pronunciation, or engage in conversation—based on what you say.

**Core Experience:**
1. Tap the glowing orb
2. Ask a question (e.g., "How do I say thank you?")
3. Carmen responds with teaching in Spanish
4. Speak back to practice
5. Continue naturally

---

## System Architecture
```
User Voice Input
      ↓
OpenAI Whisper (Speech-to-Text)
      ↓
AI Agent (GPT-4) ← Makes autonomous teaching decisions
      ↓
OpenAI TTS (Text-to-Speech)
      ↓
User Hears Spanish Response
```
---

## Tech Stack 
- **n8n** - Workflow orchestration
- **OpenAI Whisper** - Voice transcription
- **GPT-4 AI Agent** - Autonomous decision-making
- **OpenAI TTS** - Spanish voice synthesis
- **Vanilla JS** - Frontend with audio visualization

---

## n8n Workflow

<img width="868" height="371" alt="Screenshot 2025-11-19 at 9 44 32 PM" src="https://github.com/user-attachments/assets/70232b9c-dc40-42d0-bfe6-5c0df8a0a4c3" />

---

## User Interface

<img width="1462" height="740" alt="Screenshot 2025-11-19 at 9 45 18 PM" src="https://github.com/user-attachments/assets/3f87ff25-0d9c-41b6-937b-c85e48c6154e" />

<img width="1466" height="741" alt="Screenshot 2025-11-19 at 9 45 27 PM" src="https://github.com/user-attachments/assets/9f18d75b-2956-4280-b296-e8b4080d82db" />
