# 🎓 AcadAI — Student Management System

A full-stack web application to manage students and their academic results with CSV upload support, dashboards, automatic CGPA calculation, and an integrated **Agentic AI Academic Assistant** powered by RAG, persistent memory, and real-world automation.

---

## 🚀 Features

- 📂 Upload students & results via CSV
- 🎯 Automatic total, percentage & CGPA calculation
- 👨‍🎓 Student Dashboard & 👩‍🏫 Teacher Dashboard
- 🔐 Authentication (JWT-based)
- 📁 MVC Architecture
- 🤖 AI-Powered Academic Assistant (Groq API + LLaMA 3.3 70B)
- 🧠 RAG System (ChromaDB + Sentence Transformers) — AI answers from real academic documents
- 💾 Persistent Student Memory (mem0 + ChromaDB) — AI remembers past conversations and personalizes responses
- 🤝 Agentic Teacher Assistant — teacher can schedule meetings in natural language, AI notifies all students via email automatically
- 📧 Automated Email Notifications via Activepieces (no manual sending)

---

## 🛠 Tech Stack

| Layer | Tech |
|-------|------|
| Backend | Node.js, Express.js |
| AI Microservice | Python, FastAPI |
| AI Model | Groq API (LLaMA 3.3 70B Versatile) |
| Vector Database | ChromaDB |
| Embeddings | Sentence Transformers (all-MiniLM-L6-v2) |
| Student Memory | mem0 |
| Automation | Activepieces Cloud (webhook → Gmail) |
| Database | MongoDB (async via Motor) |
| Frontend | EJS, Tailwind CSS |
| Other | Multer, CSV Parser, httpx |

---

## 📂 Project Structure

```
student-management-system/
 ├── chatbot/                   # Python FastAPI AI Microservice
 │   ├── chatbot.py             # Main FastAPI app (/chatt and /tchatt endpoints)
 │   ├── rag_setup.py           # RAG system: ChromaDB + embeddings
 │   ├── docs/                  # Academic documents (txt files for RAG)
 │   │   ├── exam_schedule.txt
 │   │   ├── academic_calendar.txt
 │   │   ├── student_guidelines.txt
 │   │   └── java_oop_notes.txt
 │   ├── chroma_store/          # Auto-generated vector store (do not edit)
 │   ├── mem0_db/               # Auto-generated memory store (do not edit)
 │   ├── requirements.txt
 │   └── .env                   # GROQ_API_KEY, ACTIVEPIECES_WEBHOOK_URL
 ├── server/                    # Node.js Express Backend
 │   ├── controller/
 │   ├── database/
 │   ├── middleware/
 │   ├── model/
 │   └── routes/
 ├── views/                     # EJS Frontend
 │   ├── student/
 │   ├── teacher/
 │   └── include/
 ├── Server.js
 └── package.json
```

---

## ⚙️ How to Run Locally

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/NandaniJS08/student-management-system.git
cd student-management-system
```

### 2️⃣ Install Node Dependencies

```bash
npm install
```

### 3️⃣ Install Python Dependencies

```bash
cd chatbot
pip install -r requirements.txt
```

> ⚠️ First run downloads the embedding model (~80MB). Cached after that.

### 4️⃣ Create `.env` Files

Root `.env`:
```
PORT=3000
MONGO_URI=mongodb://localhost:27017/student_mgm2
JWT_SECRET=your_secret_key
```

`chatbot/.env`:
```
GROQ_API_KEY=your_groq_api_key
ACTIVEPIECES_WEBHOOK_URL=your_activepieces_webhook_url
```

### 5️⃣ Start MongoDB

Make sure MongoDB is running locally.

### 6️⃣ Run Both Servers

Terminal 1 — Node.js:
```bash
node Server.js
```

Terminal 2 — Python AI microservice:
```bash
cd chatbot
python -m uvicorn chatbot:app --reload
```

### 7️⃣ Open in Browser

```
http://localhost:3000
```

---

## 🧠 How the RAG System Works

```
docs/*.txt  →  chunked  →  embedded  →  stored in ChromaDB
                                               ↓
student asks question  →  question embedded  →  similar chunks retrieved
                                               ↓
                              chunks injected into Groq prompt
                                               ↓
                           AI answers using your actual documents
```

Drop any `.txt` file into `chatbot/docs/` and restart the Python server — it auto-loads everything.

---

## 💾 How Persistent Memory Works

```
Student asks a question
        ↓
mem0 searches past conversation memory for this student
        ↓
Relevant memories injected into system prompt
        ↓
AI personalizes response based on what it already knows
        ↓
After reply → new conversation saved to mem0 memory
```

Each student has their own memory. The AI remembers struggles, topics covered, and preferences across sessions.

---

## 🤖 How the Agentic Meeting Scheduler Works

```
Teacher types: "Schedule a meeting Monday 10am about Java exam"
        ↓
Groq reads the message + sees available tools (arrange_meeting)
        ↓
Groq decides to use the tool — extracts date, time, topic
Groq also writes the professional HTML email body itself
        ↓
FastAPI fetches all student emails from MongoDB
        ↓
Sends each student's data to Activepieces webhook via HTTP
        ↓
Activepieces delivers Gmail to every student automatically
        ↓
Teacher sees: "✅ Done! Notified 30 students via email."
```

No hardcoded templates. Groq writes the email. Activepieces sends it. Fully agentic.

---

## 🧪 Sample Usage

1. Upload student and result data using the CSV files in the repo
2. View dashboards for performance analysis
3. Ask the student AI assistant about exam schedules, unit topics, attendance rules, or performance
4. As a teacher, type "Schedule a meeting Thursday 2pm about semester results" — all students get notified via email automatically

---

## 📈 Future Improvements

- 📊 Charts & analytics dashboard
- 🌐 Deploy to cloud (Render / Railway)
- 📱 Improved UI/UX
- 📎 Teacher document upload via portal
- 🔔 WhatsApp notifications via Twilio

---

## 👩‍💻 Authors

- **Nandani J Solgama** — Node.js backend, frontend, database
- **Veer (VeerTheSani)** — AI microservice, RAG system, mem0 memory, agentic scheduling, Groq API integration

---

## ⭐ Support

If you like this project, give it a ⭐ on GitHub!
