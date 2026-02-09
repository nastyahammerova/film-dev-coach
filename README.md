# film-dev-coach
AI-assisted film development timer with context-aware safety constraints

<p align="center">
 <img width="1275" height="686" alt="Screenshot at Feb 09 16-53-19" src="https://github.com/user-attachments/assets/a9b29c03-e269-4cfd-951b-245d4e74611d" />
</p>

<p align="center">
  <strong>A hands-free development timer that helps you learn — but won't let you make mistakes</strong>
</p>

<p align="center">
  <a href="#for-product-managers">For PMs</a> •
  <a href="#for-developers">For Developers</a> •
  <a href="#quick-start">Quick Start</a> •
  <a href="#features">Features</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## Background

I'm a Product Manager working in identity verification, where AI safety and trust are critical. Our ML models make high-stakes decisions — approve or reject — with zero tolerance for certain types of errors.

This project explores AI safety constraints in a different domain (which also happens to be my hobby): **film photography**.

Film development is chemistry. Wrong temperature by 2°F? Ruined photos. Wrong chemical? Instant destruction. Wrong timing? Unusable negatives.

**The question:** Could an AI assistant help photographers learn and troubleshoot — without ever suggesting something that ruins their film?

---

## The Problem

Traditional film development has three pain points:

1. **Wet hands, gloves on** — Can't use phone/tablet touchscreen
2. **Time-critical steps** — Can't look away to google something
3. **High stakes** — One mistake = irretrievably lost photographs + hours of work wasted + $20 of film

Existing solutions:
- Generic kitchen timers (no context, no guidance)
- Printed instructions (can't ask questions, no timer and notifications)
- YouTube tutorials (time sensitive - hard to follow the video)
- ChatGPT (might hallucinate dangerous advice)

**What's needed:** A hands-free assistant that provides context and answers questions — but with *zero tolerance* for suggesting process modifications.

---

## For Product Managers

### Product Decisions & Learnings

This project demonstrates several PM skills directly applicable to AI products in production:

#### 1. Context-Aware Safety Modes

The AI operates in two distinct modes with different constraints:

**🟢 Safe Mode** (No timer running)
- User is learning, preparing, or troubleshooting past results
- AI can explain concepts, answer "why" questions
- **Still refuses:** Chemical substitutions, process modifications
- **Analogy:** Staging environment, training mode, exploratory analysis

**🔴 Locked Mode** (Timer active)
- User is actively developing film
- AI can ONLY explain the current step
- AI CANNOT suggest any modifications, alternatives, or time adjustments
- **Analogy:** Production environment with live user data

**Why this matters:**
- Shows understanding of context-dependent risk
- Demonstrates ability to design constraint systems
- Proves thinking about edge cases and failure modes

**Parallel to identity verification:**
- Safe mode = Internal tools, testing, investigation
- Locked mode = Live verification flow with real users

#### 2. When AI Should Say "No"

The system uses multiple layers to prevent dangerous advice:

**Layer 1: Keyword Detection**
```javascript
const dangerousKeywords = [
  'substitute', 'instead of', 'can i use',
  'what if i', 'should i add', 'extend time',
  'reduce time', 'hotter', 'cooler'
];
```

**Layer 2: System Prompt Constraints**
- Mode-specific instructions injected into every API call
- Explicit refusal language for edge cases
- Context about current step, process, and timer status

**Layer 3: User Communication**
- Refusals are polite but firm
- Explanation of *why* AI can't help with that
- Redirect to proper resources (kit instructions)

**Product lesson:** Sometimes "I can't help with that" **IS** the most helpful answer. The design challenge is making refusal feel natural, not frustrating.

#### 3. Accessibility Through Context

Voice input/output isn't a "cool feature" — it's a **requirement** based on user context:

- **User state:** Wearing gloves, hands wet with chemistry
- **Environment:** Darkroom or kitchen, standing at sink
- **Attention:** Eyes on temperature, timer, and chemicals
- **Input method:** Keyboard (with elbow/knuckle) or voice only

**Design decisions:**
- Keyboard shortcuts for critical actions (Space = start/pause)
- Voice input for questions (hands-free)
- Voice output for responses (eyes-free)
- Single-screen layout (no scrolling needed)
- Large timer display (readable from distance)

**Lesson for ID verification:** Accessibility isn't just about disabilities. It's about designing for real-world contexts (shaky hands, poor lighting, non-native language, elderly users).

#### 4. Zero-Tolerance Error Scenarios

Some product domains have **asymmetric error costs:**

**In identity verification:**
- False positive (approve fraud) = High cost
- False negative (reject legitimate user) = Medium cost
- Need to balance both

**In film development:**
- Wrong advice = Film destroyed (HIGH cost)
- No advice = User has to look it up (LOW cost)
- **Optimal:** Be helpful where safe, refuse where risky

**Design implication:** When error cost is asymmetric and high, optimize for precision over recall. Better to refuse than to be wrong.

#### 5. Iteration Based on Real Use

Development process (PM skillset demonstrated):

**V1: Basic Timer**
- Core functionality: Step-by-step timer with agitation alerts
- Voice announcements for hands-free operation
- Keyboard shortcuts

**V2: AI Integration**
- Added educational Q&A capability
- Implemented safety constraints
- Mode switching based on timer state

**V3: Voice Interaction**
- Speech-to-text for questions (hands-free input)
- Text-to-speech for responses (eyes-free output)
- Speaker buttons on all AI responses

**V4: Optimization**
- Single-screen layout (no scrolling)
- MacBook Air-specific sizing
- Compact typography balancing

**Each iteration validated the previous one.** Shipped incrementally, learned, adjusted.

### Metrics of Success

Traditional product metrics don't apply here:

**What I'm NOT measuring:**
- ❌ Daily active users
- ❌ Retention rate
- ❌ Time in app
- ❌ Revenue

**What success looks like:**
- ✅ Zero reports of ruined film due to AI advice
- ✅ Users feel confident to ask questions
- ✅ AI refusals feel helpful, not blocking
- ✅ Hands-free operation actually works

**Parallel:** In identity verification, success isn't just "high accuracy." It's balancing fraud prevention AND user experience. You need both sides of the equation.

### What I'd Do Differently

**User Research:**
- Should have tested with actual photographers before building AI features
- Assumption: Keyboard shortcuts are sufficient
- Reality: Might need bigger buttons, different shortcuts

**Analytics:**
- Should have added (privacy-safe) telemetry
- Which steps do users pause most on?
- What questions do they ask the AI?
- Where do they adjust timings?

**Error Handling:**
- Better API failure states
- Offline mode for timer (without AI)
- Local storage for draft processes

**Templating:**
- Should have designed process steps as JSON from the start
- Would make it easier to add new film types
- Community could contribute processes

### Key PM Takeaways

1. **AI Safety Is Product Design**
   - Not just an engineering concern
   - Deciding when AI should refuse is a product decision
   - Requires understanding user context and risk

2. **Constraints Enable Trust**
   - Limiting AI capabilities made it trustworthy
   - Users know it won't lead them astray
   - Applies to ML in production: guardrails → confidence

3. **Context Changes Everything**
   - What's "helpful" in learning mode is "dangerous" in execution mode
   - Product needs to understand user state and adapt
   - One-size-fits-all AI is risky

4. **Ship and Learn**
   - Started with timer, added AI, added voice
   - Each feature validated previous decisions
   - Iteration beats perfection

---

## For Developers

### Technical Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     User Interface                      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │    Timer     │  │  AI Chat     │  │   Settings   │   │
│  │   Display    │  │  Interface   │  │   Controls   │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────┘
           │                   │                   │
           ▼                   ▼                   ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐
│  Timer Engine   │  │   AI Safety     │  │   Speech    │
│                 │  │   Constraints   │  │   APIs      │
│ • Step tracking │  │                 │  │             │
│ • Intervals     │  │ • Mode system   │  │ • STT       │
│ • Agitation     │  │ • Keyword check │  │ • TTS       │
└─────────────────┘  │ • Prompt inject │  └─────────────┘
                     └─────────────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │  Anthropic API  │
                     │  (Claude)       │
                     └─────────────────┘
```

### Key Technical Decisions

#### 1. Single-File Architecture

**Decision:** Keep everything in one HTML file

**Why:**
- Easy deployment (just open in browser)
- No build process needed
- Works offline (except AI features)
- Simple for non-technical users

**Trade-off:** Harder to maintain as it grows, but worth it for ease of use.

#### 2. Vanilla JavaScript (No Framework)

**Decision:** No React, Vue, or other frameworks

**Why:**
- Minimal dependencies
- Fast load time
- Educational (easier to read for learners)
- No build step

**Trade-off:** More verbose code, but more portable and accessible.

#### 3. Anthropic Claude API Integration

**Decision:** Use Claude API directly from browser (with CORS workaround)

**Why:**
- Best-in-class safety alignment (matches project goals)
- Strong instruction following
- Good at refusing appropriately
- Transparent pricing

**Implementation:**
```javascript
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': ANTHROPIC_API_KEY,
    'anthropic-version': '2023-06-01',
    'anthropic-dangerous-direct-browser-access': 'true'
  },
  body: JSON.stringify({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 1024,
    messages: [{ role: 'user', content: userMessage }],
    system: buildSystemPrompt()
  })
});
```

**CORS Note:** Browser security blocks API calls from `file://` URLs. Must run with local server (see setup).

#### 4. Safety Constraint System

**Multi-layered approach:**

**Layer 1: Mode Detection**
```javascript
function updateAIMode() {
  if (running) {
    aiLocked = true;
    elAIMode.innerHTML = '🔴 <strong>LOCKED MODE:</strong> Timer Active';
  } else {
    aiLocked = false;
    elAIMode.innerHTML = '🟢 <strong>SAFE MODE:</strong> Pre-Development';
  }
}
```

**Layer 2: Context Injection**
```javascript
function buildSystemPrompt() {
  const currentStep = recipe.steps[stepIndex];
  
  let systemPrompt = `You are a film development assistant.

CRITICAL SAFETY RULES:
${aiLocked ? `
🔴 LOCKED MODE - Timer is running!
- You can ONLY explain what the current step means
- You CANNOT suggest modifications, substitutions, or changes
` : `
🟢 SAFE MODE - No timer running
- You can explain concepts and troubleshoot
- You CANNOT suggest recipe modifications or substitutions
`}

CURRENT CONTEXT:
- Process: ${recipe.name}
- Current Step: ${currentStep.title}
- Timer Status: ${running ? 'RUNNING' : 'Not running'}

Keep responses concise. Never suggest deviating from the recipe.`;

  return systemPrompt;
}
```

**Layer 3: Keyword Detection**
```javascript
function checkDangerousQuestion(question) {
  const dangerousKeywords = [
    'substitute', 'instead of', 'can i use', 'replace',
    'what if i', 'should i add', 'extend time', 'reduce time'
  ];
  
  const lowerQ = question.toLowerCase();
  return dangerousKeywords.some(keyword => lowerQ.includes(keyword));
}
```

#### 5. Speech APIs

**Speech-to-Text (Voice Input):**
```javascript
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
const recognition = new SpeechRecognition();
recognition.continuous = false;
recognition.interimResults = false;
recognition.lang = 'en-US';

recognition.onresult = (event) => {
  const transcript = event.results[0][0].transcript;
  elAIInput.value = transcript;
};
```

**Text-to-Speech (Voice Output):**
```javascript
window.speakAIMessage = function(button) {
  const content = messageDiv.textContent.replace('AI Assistant:', '').trim();
  const utterance = new SpeechSynthesisUtterance(content);
  utterance.rate = 1.0;
  utterance.pitch = 1.0;
  window.speechSynthesis.speak(utterance);
};
```

### Tech Stack

- **Frontend:** Vanilla JavaScript, HTML5, CSS3
- **AI:** Anthropic Claude API (Sonnet 4)
- **Speech:** Web Speech API (browser native)
- **Fonts:** Google Fonts (DM Sans, DM Mono)
- **No build tools, no frameworks, no dependencies**

### File Structure

```
film-dev-coach/
├── film-dev-coach-AI-API-version.html  # Main application (single file)
├── README.md                            # This file
├── AI_SETUP_GUIDE.md                   # API key setup instructions
├── LICENSE                             # MIT License
└── screenshots/                        # UI screenshots
    ├── main-interface.png
    ├── ai-chat.png
    └── voice-features.png
```

---

## Features

### ⌨️ Hands-Free Operation
- **Keyboard shortcuts:** Space (start/pause), N (next), B (back), R (reset)
- **Voice input:** Click microphone, speak your question
- **Voice output:** AI responses read aloud
- **No scrolling:** Everything fits on one screen

### 🤖 AI Assistant with Safety Rails
- **Explains concepts:** "Why is 102°F critical?"
- **Troubleshoots results:** "My negatives are too dark, what went wrong?"
- **Refuses dangerous advice:** Won't suggest substitutions or modifications
- **Context-aware:** Knows which step you're on, timer status

### 🔒 Safety Modes
- **🟢 Safe Mode:** Educational Q&A before/after development
- **🔴 Locked Mode:** Instruction-only during active timer
- **Auto-switching:** Modes change based on timer state

### 🎬 Film Processes Included
- **Chemical Preparation:** Step-by-step mixing of developer and blix
- **C-41 Development:** Complete Cinestill process with agitation schedules
- **Pre-rinse, Developer, Blix, Wash, Photo-Flo, Drying**

### 📱 Optimized for MacBook Air M1
- **Single-screen layout:** No scrolling on 1440x900 display
- **Readable from distance:** Large timer, clear typography
- **Compact but complete:** All info visible at once

---

## Quick Start

### Prerequisites

- Modern web browser (Chrome, Safari, Firefox, Edge)
- Python 3 (for local server) OR VS Code with Live Server
- Anthropic API key (optional, for AI features)

### Installation

#### 1. Clone the Repository

```bash
git clone https://github.com/nastyahammerova/film-dev-coach
cd film-dev-coach
```

#### 2. Run Local Server

**Option A: Python (Easiest)**
```bash
python3 -m http.server 8000
```

**Option B: VS Code Live Server**
1. Install "Live Server" extension
2. Right-click HTML file → "Open with Live Server"

**Option C: Node.js**
```bash
npm install -g http-server
http-server
```

#### 3. Open in Browser

Navigate to: `http://localhost:8000`

**That's it!** The timer works immediately. AI features require additional setup.

### Enabling AI Assistant (Optional)

#### 1. Get Anthropic API Key

- Go to [console.anthropic.com](https://console.anthropic.com)
- Sign up / Log in
- Create an API key
- Copy the key (looks like `sk-ant-api03-...`)

#### 2. Add API Key to File

Open `film-dev-coach-AI-API-version.html` in a text editor:

```javascript
// Find this section (around line 860):
const ANTHROPIC_API_KEY = 'YOUR_API_KEY_HERE';  // ← Paste your key here
const API_AVAILABLE = false;                     // ← Change to: true
```

#### 3. Save and Reload

- Save the file
- Refresh browser (`Command+R` or `F5`)
- Click "🤖 AI Assistant" button
- Ask a question!

**Detailed setup guide:** See [AI_SETUP_GUIDE.md](AI_SETUP_GUIDE.md)

---

## Usage

### Basic Timer Operation

1. **Select process:** Choose "Chemical Preparation" or "C-41 Development"
2. **Read current step:** Instructions show automatically
3. **Start timer:** Click "Start" or press `Space`
4. **Follow agitation prompts:** Voice and visual alerts guide you
5. **Navigate steps:** "Next →" / "Previous ←" buttons or `N` / `B` keys

### Using AI Assistant

1. **Click** "🤖 AI Assistant" button to open chat
2. **Ask questions:**
   - Type and click "Send"
   - OR click 🎤 microphone and speak
3. **Get responses:**
   - Read the text
   - OR click 🔊 speaker to hear it read aloud
4. **Check mode indicator:**
   - 🟢 Safe Mode = Educational Q&A
   - 🔴 Locked Mode = Current step explanations only

### Keyboard Shortcuts

- `Space` — Start/Pause timer
- `N` — Next step
- `B` — Back (previous step)
- `R` — Reset current step
- `Enter` — Send AI message (when in text field)

---

## API Costs

**Typical usage:**
- 10-20 questions during development session
- ~500 tokens input per question
- ~300 tokens output per response
- **Total:** ~$0.01 - $0.05 per session

**Anthropic pricing (Claude Sonnet 4):**
- Input: $3 per million tokens
- Output: $15 per million tokens

**$5 in credits = 100+ development sessions**

---

## Browser Compatibility

### Tested & Working:
- ✅ Chrome 90+ (macOS, Windows, Linux)
- ✅ Safari 14+ (macOS, iOS)
- ✅ Firefox 88+ (macOS, Windows, Linux)
- ✅ Edge 90+ (Windows)

### Speech Features:
- ✅ Chrome/Edge: Full support (STT + TTS)
- ✅ Safari: Full support (STT + TTS)
- ⚠️ Firefox: TTS works, STT limited
- ❌ Older browsers: May not support Web Speech API

---

## Contributing

Contributions are welcome! This project can benefit from:

### For Product Managers:
- **User research:** Test with actual film photographers
- **Process documentation:** Add more film types (B&W, E-6, etc.)
- **UX improvements:** Better error states, loading indicators
- **Documentation:** Improve setup guides, add FAQs

### For Developers:
- **Code quality:** Refactoring, better separation of concerns
- **Features:** Offline mode, process templates, import/export
- **Testing:** Unit tests, integration tests, E2E tests
- **Performance:** Optimize rendering, reduce bundle size

### For Film Photographers:
- **Recipes:** Contribute accurate development processes
- **Testing:** Report bugs, suggest improvements
- **Education:** Write guides for beginners

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch:** `git checkout -b feature/your-feature`
3. **Make your changes**
4. **Test thoroughly**
5. **Commit:** `git commit -m 'Add your feature'`
6. **Push:** `git push origin feature/your-feature`
7. **Open a Pull Request**

---

## Roadmap

### V1.0 (Current)
- ✅ Core timer with step navigation
- ✅ AI assistant with safety modes
- ✅ Voice input/output
- ✅ Cinestill C-41 process
- ✅ Chemical preparation guide

### V1.1 (Next)
- [ ] UI Improvement (Add Next Step on the screen)
- [ ] Offline mode (timer works without internet)
- [ ] Local storage for custom processes
- [ ] Import/export process templates (JSON)
- [ ] Dark/light theme toggle

### V1.2 (Future)
- [ ] Black & white film processes (D-76, HC-110, etc.)
- [ ] E-6 slide film process
- [ ] Push/pull development calculations
- [ ] Temperature compensation tables

### V2.0 (Long-term)
- [ ] Mobile app (React Native or PWA)
- [ ] Multi-language support
- [ ] Community process library
- [ ] Analytics (privacy-safe, opt-in)

---

## FAQ

### General

**Q: Do I need an API key to use this?**  
A: No! The timer works perfectly without AI. The AI assistant is optional.

**Q: How much does the API cost?**  
A: About $0.01-0.05 per development session. $5 in credits = 100+ sessions.

**Q: Can I use this offline?**  
A: Timer works offline. AI features require internet (for now).

**Q: Is my API key secure?**  
A: It's stored in the HTML file on your computer. Don't share the file with your key in it.

### Technical

**Q: Why do I need a local server?**  
A: Browsers block API calls from `file://` URLs for security. Local server makes it `http://localhost`.

**Q: Can I use a different AI model?**  
A: Yes! Edit the `model` parameter in the code. But test safety constraints carefully.

**Q: Does this work on mobile?**  
A: Yes, but designed for desktop/laptop. Mobile UI could be better (contributions welcome!).

**Q: Why single HTML file?**  
A: Simplicity. Easy to deploy, share, and run. No build process needed.

### Film Photography

**Q: Does this work for black & white film?**  
A: Not yet. Only C-41 color negative currently. B&W coming soon (contributions welcome!).

**Q: Can I add my own chemistry?**  
A: Yes! Edit the `PRESETS` object to add your process.

**Q: Is the Cinestill process accurate?**  
A: Yes, tested with Cinestill C-41 kit. Always verify with your kit's instructions.

---

## License

MIT License - See [LICENSE](LICENSE) file for details.

**TL;DR:** Free to use, modify, distribute. Attribution appreciated but not required.

---

## Acknowledgments

- **Film photography community** - For feedback and process validation
- **Contributors** - Everyone who's helped improve this project

---

## About

Built by a Product Manager exploring AI safety constraints in a different domain.

**Professional background:** Identity verification, ML product management, trust & safety

**Why film photography?** First of all - my hobby. Also - high-stakes, precise processes with zero tolerance for errors — perfect testbed for AI safety patterns.


---

## Contact

- **GitHub Issues:** For bugs, features, questions
- **LinkedIn:** Anastasia Molotkova(linkedin.com/in/anastasiamolotkova)
- **Email:** nastya.hammerova@gmail.com

---

<p align="center">
  <strong>Star ⭐ this repo if you find it useful!</strong><br>
  Built with ☕ and a love for film photography
</p>

---

*Last updated: February 2026*
