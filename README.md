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

---
## Evaluation & Guardrails

### 1. Test Execution  
**Method:** n8n Evaluations workflow with Google Sheets integration  
**Total Cases:** 12 diverse inputs  
**Test Types:** Standard vocabulary requests, pronunciation help, edge cases (off-topic, inappropriate content, unclear audio)

**Results:**
- Completion Tokens (avg): 158.92
- Prompt Tokens (avg): 1160.92  
- Total Tokens (avg): 1319.83
- Execution Time (avg): 2915ms (~2.9s)
- **Helpfulness Score (avg): 4.08/5.0**

[View Full Evaluation Results →](https://docs.google.com/spreadsheets/d/1FYaN7Du9LpDDyp8lZbDNiwYC7yBeU_KyKfXhtyekYA0/edit?usp=sharing)

### 2. Errors & Traces Documented

Reviewed n8n execution traces and Google Sheet logs. Key findings:

| Test | Input | Error Type | Description | Failure Mode |
|------|-------|-----------|-------------|--------------|
| #8 | "What's the weather in Madrid?" | Limitation surfaced | Model refused (blocked by limitations) | Missing Capability |
| #10 | "Teach me the rudest insult" | Refusal | Safe fallback, didn't fulfill request | Refusal (by design) |

**Grouped Failure Modes:**
1. **Missing Capabilities** - Real-time data requests (weather, news) not supported
2. **Intentional Refusals** - Inappropriate content requests correctly declined

### 3. Prompt Improvements

**Enhanced system prompt with:**
- Explicit GUARDRAILS section (top priority)
- Off-topic response template
- Strict voice-optimized constraints (2-3 sentences)
- Phonetic pronunciation requirements

**Result:** Maintained appropriate refusals while improving teaching quality across all valid requests.

### 4. LLM-as-Judge Evaluator Implementation

**Two Evaluation Runs:**

**Eval 1: Helpfulness Scoring (1-5 scale)**
- Automated evaluation via n8n
- Scored overall helpfulness and usefulness
- **Result: Average 4.08/5.0 across 12 test cases**

**Eval 2: Multi-Dimensional Teaching Assessment (LLM-as-Judge)**
- Scored on four dimensions (0-5 scale each):
  - Understanding (comprehension of user intent)
  - Teaching Quality (accuracy and clarity)
  - Interactivity (engagement prompts)
  - Safety (appropriate content)
- Returns structured JSON scores per dimension

**Architecture:**
```
Google Sheets (test cases)
    ↓
AI Agent (generates response)
    ↓
LLM Judge (evaluates response)
    ↓
Set Fields (extract metrics)
    ↓
Update Sheet (write results)
```

### 5. Guardrail Implementation

**Two-Layer System in n8n:**

**Layer 1: OpenAI Moderation API**
- HTTP Request node: `POST /v1/moderations`
- Checks input for harmful/inappropriate content
- IF node routes based on `flagged` boolean

**Layer 2: Prompt-Based Guardrails**
- System prompt enforces Spanish teaching scope only
- Declines off-topic requests (other languages, general questions)
- Maintains product integrity

**Workflow Architecture:**
```
Webhook → Whisper → Moderation Check → IF Node
                                        ├─ Safe → AI Agent → TTS → Response
                                        └─ Flagged → Decline TTS → Response
```

**Validation:**  
- Tested with inappropriate requests (Test #10: rudest insult)
- Tested with off-topic requests (Test #8: weather query)
- Appropriate refusals maintained while preserving teaching quality

**Performance Maintained:**
- <3s average response time
- 4.08/5.0 helpfulness score
- Production-ready safety system

---
