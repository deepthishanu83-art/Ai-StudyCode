# AI StudyMate — Personal AI Teacher Frontend

A modern, responsive React frontend for **AI StudyMate**, an AI-powered personal teacher designed for school and college students.

Styled with a strict black and dark-gray palette (`#0B0B0C` background, `#151619` panels, `#303238` borders, white/light-gray text) matching the reference design.

---

## 🚀 Features

### 1. Left Sidebar
- **Logo & Branding:** AI StudyMate ("Your Personal AI Teacher") with graduation icon.
- **New Chat Action:** Instantly create a new study session.
- **Navigation:**
  - `Dashboard`
  - `AI Teacher` (active session)
  - `My Notes`
  - `Quiz Generator` (opens MCQ Quiz modal)
  - `Question Bank` (opens 1, 2, and 5-mark revision questions modal)
  - `Voice Tutor` (hands-free speech tutor)
  - `Live Tutor`
- **Independently Scrollable Chat History:**
  - Lists sessions with title and relative timestamps.
  - Clicking any history item restores the full conversation.
  - Hover action buttons to rename or delete conversations.
- **User Profile Footer:** Displays current student profile ("Achu / Student") and settings gear.
- **Mobile Responsive:** Collapsible drawer with smooth backdrop overlay.

### 2. Main Chat Area
- **Header:**
  - Conversation title with inline pencil rename trigger.
  - Message count & date/time indicator.
  - In-chat search filter.
  - `Clear Chat` action with confirmation.
  - Right panel toggle button & mobile hamburger toggle.
- **Welcome / Hero Card:**
  - ChatGPT-style introduction banner with friendly mascot and 4 quick suggestion chips.
- **Independently Scrollable Messages:**
  - **AI Teacher Messages:** Formatted markdown, bold key terms, styled bullet points, dark monospace chemical/mathematical formula blocks, timestamp, copy button, and Text-To-Speech (TTS) read aloud.
  - **User Messages:** Right-aligned bubbles with attachment tags, timestamp, and read receipts (`✓✓`).
  - **Thinking / Loading State:** 3-dot pulsing indicator while awaiting answers.
- **Fixed Composer:**
  - Working **`+` Button** opening the dark Attachment Menu.
  - Auto-resizing textarea: `Enter` to send, `Shift + Enter` for new lines.
  - Speech Recognition (Web Speech API) with live pulse indicator.
  - Send button (automatically disabled if input and attachments are empty).
  - Selected file attachment chips with remove (`×`) buttons.

### 3. Working Attachment Menu (Plus Menu)
- **Upload Image:** Hidden input for `.png, .jpg, .jpeg, .webp, .gif`.
- **Upload PDF:** Hidden input for `.pdf` documents.
- **Upload TXT:** Hidden input for `.txt, .md, .csv, .json`.
- **Paste Text:** Opens a dark modal to paste raw notes, formulas, or syllabus chapters directly into the active session.
- Validates file types and sizes (< 15MB).
- Keyboard accessible (`Esc` closes, click outside closes).

### 4. Right Tools & Files Panel
- **Tabs:** `Files`, `Notes`, `Quick Actions`.
- **Files Tab:**
  - Lists uploaded session files with file format badges (PDF, TXT, IMG), file size, and "Ready" status badge.
  - Remove / delete file actions.
- **Notes Tab:**
  - Interactive student scratchpad.
  - Auto-persists notes to `localStorage`.
  - "Send to Chat" button to query the AI directly from written notes.
  - "Export" button to download notes as a `.md` markdown file.
- **Quick Actions Tab:**
  - Quick triggers for Quiz Generator, Question Bank, Answer from Notes, Summarize Key Concepts, and Step-by-Step explanations.
- **Need Help with Voice? Card:** Quick link to hands-free voice tutor mode.
- **Pro Tip Card:** Dismissible student study tip.
- Collapsible on desktop and default-collapsed on tablet/mobile screens.

### 5. Persistence
- Prototype state stored automatically in `localStorage`:
  - Conversations & messages: `ai_studymate_conversations_v1`
  - Uploaded files: `ai_studymate_files_v1`
  - Notes scratchpad: `ai_studymate_notes_v1`

---

## 🛠️ Tech Stack
- **Framework:** React 18 + Vite 6
- **Language:** JavaScript (ESNext)
- **Styling:** Vanilla CSS with custom tokens and responsive media queries
- **Icons:** `lucide-react`
- **Network:** Native Fetch API (`src/services/api.js`)

---

## 🔌 Backend Integration & API Contracts

The frontend connects via `src/services/api.js`. You can set the backend URL using an environment variable:
```env
VITE_API_BASE_URL=http://localhost:5000
```

### Endpoints Specification:

#### 1. `POST /api/chat`
- **Request Body:**
  ```json
  {
    "message": "Can you explain the role of chlorophyll?",
    "conversationId": "conv-1",
    "attachments": [
      { "name": "Photosynthesis_Notes.pdf", "type": "pdf" }
    ]
  }
  ```
- **Expected Response (200 OK):**
  ```json
  {
    "reply": "Photosynthesis is the process...",
    "equation": "6CO₂ + 6H₂O ──> C₆H₁₂O₆ + 6O₂",
    "suggestions": ["Explain Calvin Cycle", "Generate Quiz"]
  }
  ```

#### 2. `POST /api/upload`
- **Request Body:** `multipart/form-data` with key `file`.
- **Expected Response (200 OK):**
  ```json
  {
    "fileId": "file-123",
    "name": "Biology_Notes.txt",
    "size": "856 KB",
    "type": "txt",
    "status": "Ready"
  }
  ```

#### 3. `POST /api/quiz`
- **Request Body:**
  ```json
  {
    "topic": "Photosynthesis - Class 10",
    "notes": "Optional notes text"
  }
  ```
- **Expected Response (200 OK):**
  ```json
  {
    "questions": [
      {
        "id": 1,
        "question": "Which organelle absorbs light?",
        "options": ["Chloroplast", "Mitochondria"],
        "answer": 0,
        "explanation": "Chloroplasts contain chlorophyll."
      }
    ]
  }
  ```

> **Offline / Missing Backend Handling:**
> If the backend server is not running or endpoints are unreachable, `api.js` captures the error gracefully and provides contextual study responses and quizzes so you can develop and test the entire interface without errors.

---

## 📦 Installation and Run Commands

```powershell
# 1. Install all dependencies
npm install

# 2. Run BOTH Frontend & Backend together with a single command
npm run dev:all

# Alternatively, run them in separate terminals:
# Terminal 1: Backend Server (Port 5000)
npm run server

# Terminal 2: Vite Frontend (Port 5173)
npm run dev

# 3. Build for production (verified with 0 errors)
npm run build

# 4. Preview production build
npm run preview
```

---

## ✅ Testing Checklist

| Test Item | Action | Expected Result |
| :--- | :--- | :--- |
| **Strict Dark Theme** | Inspect background & panels | Strict black `#0B0B0C`, panels `#151619`, borders `#303238` |
| **Sidebar Navigation** | Click "New Chat" | A new conversation is created and activated |
| **History Restore** | Click any item in Chat History | Conversation restored with full messages & timestamps |
| **History Rename** | Click pencil icon on history item | Inline input allows renaming conversation title |
| **History Delete** | Click trash icon on history item | Prompts confirmation and removes conversation |
| **Plus Menu Toggle** | Click `+` button in composer | Dark popup appears with Image, PDF, TXT, Paste options |
| **Attachment Selection** | Choose a file via Image/PDF/TXT | Chip appears in composer with format badge and `×` button |
| **Paste Notes Modal** | Click "Paste Text" in `+` menu | Modal opens; submitting adds text file and sends note reference |
| **Send Message** | Type message and hit `Enter` | User message right-aligned, loading dots appear, AI response rendered |
| **Shift+Enter** | Press `Shift + Enter` in composer | Creates a new line in textarea without sending |
| **Voice Tutor** | Click microphone icon | Web Speech recognition initiates; clicking voice card reads aloud |
| **Right Panel Files** | View Uploaded Files tab | Shows PDF and TXT with size, "Ready" pill, and delete button |
| **Notes Scratchpad** | Type notes in Notes tab | Notes persist in `localStorage` and can be exported as `.md` |
| **Quiz Modal** | Click "Generate Quiz" | Interactive 3-question MCQ opens with options, scoring, and explanations |
| **Mobile Responsiveness**| Resize browser to < 768px | Sidebar collapses into drawer; hamburger button toggles menu |
