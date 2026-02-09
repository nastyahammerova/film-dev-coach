# How to Enable the AI Assistant

## What You'll Need
- The film-dev-coach-AI-API-version.html file
- A text editor (Notepad, TextEdit, RStudio, VS Code, or any editor)
- An Anthropic API key (free to get)

---

## Step 1: Get an Anthropic API Key

### 1.1 Create an Account

1. **Go to** https://console.anthropic.com
2. **Click** "Sign up" (or "Log in" if you have an account)
3. **Complete registration:**
   - Enter your email
   - Verify your email
   - Set a password

### 1.2 Add Credits to Your Account

1. **Click** "Settings" or "Billing" in the sidebar
2. **Add credits** - minimum is usually $5
3. **Enter payment information**

💡 **Cost:** The AI uses very little - asking 10-20 questions costs about $0.01-0.05 per session. $5 in credits = 100+ development sessions.

### 1.3 Create Your API Key

1. **Go to** "API Keys" section
2. **Click** "Create Key"
3. **Copy the API key** that appears
   - It looks like: `sk-ant-api03-...`
   - **Important:** Copy it immediately - you won't see it again!
4. **Save it** somewhere safe temporarily (you'll paste it into the file next)

✅ **You now have an API key!**

---

## Step 2: Edit the HTML File

### 2.1 Open the File in a Text Editor

**Option A: RStudio (Recommended if you have it)**
1. **Open RStudio**
2. **File** → **Open File...**
3. **Navigate to** `film-dev-coach-AI-API-version.html`
4. **Click** "Open"

**Option B: TextEdit (Mac)**
1. **Right-click** the HTML file
2. **Open With** → **TextEdit**
3. **Important:** If TextEdit shows weird formatting:
   - Go to **Format** menu → **Make Plain Text**

**Option C: Notepad (Windows)**
1. **Right-click** the HTML file
2. **Open With** → **Notepad**

**Option D: VS Code (If you have it)**
1. **Right-click** the HTML file
2. **Open With** → **Visual Studio Code**

### 2.2 Find the Configuration Section

Look for this section near the top of the file (around line 860):
```javascript
// ╔════════════════════════════════════════════════════════════════╗
// ║                                                                ║
// ║              🔑 PASTE YOUR API KEY HERE 🔑                    ║
// ║                                                                ║
// ║  1. Get key from: https://console.anthropic.com               ║
// ║  2. Replace the text below with your key                      ║
// ║  3. Change "false" to "true" on the next line                 ║
// ║  4. Save this file                                            ║
// ║                                                                ║
// ╚════════════════════════════════════════════════════════════════╝

const ANTHROPIC_API_KEY = 'YOUR_API_KEY_HERE';  // ← Paste your key here (between the quotes)
const API_AVAILABLE = false;                     // ← Change this to: true
```

**Can't find it?** Use the search function:
- **Mac:** Press `Command + F`
- **Windows:** Press `Ctrl + F`
- **Search for:** `YOUR_API_KEY_HERE`

### 2.3 Make the Changes

**Step 1:** Replace `YOUR_API_KEY_HERE` with your actual API key:

**Before:**
```javascript
const ANTHROPIC_API_KEY = 'YOUR_API_KEY_HERE';
```

**After:**
```javascript
const ANTHROPIC_API_KEY = 'sk-ant-api03-your-actual-key-here';
```

**Step 2:** Change `false` to `true`:

**Before:**
```javascript
const API_AVAILABLE = false;
```

**After:**
```javascript
const API_AVAILABLE = true;
```

### 2.4 Save the File

- **Mac:** Press `Command + S`
- **Windows:** Press `Ctrl + S`
- Or: **File** → **Save**

✅ **Configuration complete!**

---

## Step 3: Run with a Local Server

**Why?** Browsers block API calls from files opened directly (file://) for security. You need to run a simple web server.

### Option 1: Python (Easiest - Works on Mac/Windows/Linux)

#### 3.1 Open Terminal/Command Prompt

**Mac:**
1. Press `Command + Space`
2. Type "Terminal"
3. Press Enter

**Windows:**
1. Press `Windows Key`
2. Type "Command Prompt" or "cmd"
3. Press Enter

#### 3.2 Navigate to Your Folder

Type `cd ` (with a space after cd), then drag your folder into the Terminal window, then press Enter.

**Example:**
```bash
cd ~/Documents/FilmDev
```

Or navigate manually:
```bash
cd Documents
cd FilmDev
```

#### 3.3 Run the Server

Copy and paste this command:
```bash
python3 -m http.server 8000
```

If that doesn't work, try:
```bash
python -m http.server 8000
```

**You should see:**
```
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

✅ **Server is running!** Keep this Terminal window open.

#### 3.4 Open in Browser

1. **Open your browser** (Chrome, Safari, Firefox)
2. **Type in the address bar:** `localhost:8000`
3. **Press Enter**
4. **Click** on your HTML file

---

### Option 2: VS Code Live Server

#### 3.1 Install VS Code

If you don't have it: Download from https://code.visualstudio.com

#### 3.2 Install Live Server Extension

1. **Open VS Code**
2. **Click** Extensions icon (left sidebar) or press `Command + Shift + X`
3. **Search for:** "Live Server"
4. **Install** the extension by Ritwick Dey

#### 3.3 Open and Run

1. **Right-click** your HTML file
2. **Open With** → **Visual Studio Code**
3. **Right-click** anywhere in the file
4. **Select** "Open with Live Server"

Browser opens automatically!

---

### Option 3: Node.js http-server

If you have Node.js installed:
```bash
npm install -g http-server
cd path/to/your/folder
http-server
```

Then open: http://localhost:8080

---

## Step 4: Test the AI Assistant

### 4.1 Open the AI Chat

1. **Click** the "🤖 AI Assistant" button at the top
2. The chat panel should open

### 4.2 Ask a Test Question

Type or speak:
```
Why is 102°F so critical for C-41 development?
```

**If it works:**
- You'll see a response from the AI
- It will explain the temperature importance

**If you see an error:**
- See Troubleshooting section below

### 4.3 Try Voice Features

**Voice Input (🎤):**
1. **Click** the microphone button
2. **Allow** microphone access when browser asks
3. **Speak** your question
4. Text appears automatically

**Voice Output (🔊):**
1. **Click** the speaker button on any AI response
2. Listen to it read aloud

---

## Troubleshooting

### "Failed to fetch" or "CORS error"

**Problem:** You're opening the file directly (double-clicking)

**Solution:** Use a local server (see Step 3 above)

---

### "API error: 401"

**Problem:** Your API key is wrong or invalid

**Solutions:**
1. Go back to Anthropic console
2. Check you copied the entire key (starts with `sk-ant-api03-`)
3. Make sure there are no spaces before or after the key
4. Try creating a new API key

---

### "API error: 429"

**Problem:** You've used up your credits or hit rate limit

**Solutions:**
1. Go to Anthropic console → Billing
2. Check your credit balance
3. Add more credits if needed
4. Wait a few minutes if you've been making many requests

---

### "API error: 404 - model not found"

**Problem:** The model name in the code is outdated

**Solution:** Make sure the code has this model name:
```javascript
model: 'claude-sonnet-4-20250514',
```

If it's different, update it in the code (search for `model:`).

---

### "Microphone not working"

**Problem:** Browser doesn't have microphone permission

**Solutions:**

**Chrome:**
1. Click the 🔒 lock icon in address bar
2. Change Microphone to "Allow"
3. Reload the page

**Safari:**
1. Safari menu → Settings → Websites
2. Microphone → Allow for localhost

**Firefox:**
1. Click 🔒 in address bar
2. Permissions → Microphone → Allow

---

### "Network error" or "Connection refused"

**Problem:** Internet connection or firewall blocking API

**Solutions:**
1. Check your internet connection
2. Try disabling VPN temporarily
3. Check firewall settings
4. Try a different network

---

### Still Not Working?

1. **Check console for errors:**
   - Press `F12` (or `Command + Option + I` on Mac)
   - Look for red error messages
   - Share these if asking for help

2. **Verify your setup:**
   - API key starts with `sk-ant-api03-`
   - `API_AVAILABLE = true` (not false)
   - Running through localhost (not file://)
   - Have internet connection
   - Have credits in Anthropic account

---

## Privacy & Security

### Your API Key is Safe

✅ **Stored locally** - Only in the HTML file on your computer  
✅ **Not uploaded** - Unless you manually share the file  
✅ **Only sent to Anthropic** - When making AI requests  
✅ **Can be revoked** - Delete it anytime from Anthropic console  

### Best Practices

- ⚠️ **Never share the HTML file** with your API key in it
- ⚠️ **Don't commit to GitHub** with your real key
- ⚠️ **Delete and create new key** if accidentally shared
- ✅ **Use placeholder** when sharing code

### Removing Your Key

If you want to share the file:

1. Open the HTML file
2. Replace your key with: `YOUR_API_KEY_HERE`
3. Change `true` back to `false`
4. Save
5. Now it's safe to share

---

## Usage Costs

### How Much Does It Cost?

**Typical session:**
- Ask 10-20 questions during film development
- Average ~500 input tokens per question
- Average ~300 output tokens per response
- **Cost: $0.01 - $0.05 per session**

**Anthropic Pricing (Claude Sonnet 4):**
- Input: $3 per million tokens
- Output: $15 per million tokens

**Translation:**
- $5 in credits = 100+ development sessions
- Very affordable for occasional use

### Monitoring Your Usage

1. **Go to** console.anthropic.com
2. **Check** "Usage" section
3. **See** how many credits remain

---

## Using Without AI

**Good news:** You don't need AI to use the timer!

**Without API key:**
- ✅ Timer works perfectly
- ✅ Voice announcements work
- ✅ Keyboard shortcuts work
- ✅ All step instructions visible
- ❌ Can't ask AI questions
- ❌ No interactive chat

**Just:**
1. Open the HTML file in browser
2. Use the timer normally
3. Follow the printed instructions

The AI is a helpful extra, not required!

---

## Updating the Code

### To Change the Model

If you want to use a different Claude model:

1. **Find** this line in the code:
```javascript
model: 'claude-sonnet-4-20250514',
```

2. **Replace** with a different model:
```javascript
model: 'claude-opus-4-20251101',  // More capable, more expensive
// or
model: 'claude-haiku-4-20251001',  // Faster, cheaper
```

3. **Save** and reload

### To Add More Processes

Edit the `PRESETS` object in the code to add B&W, E-6, or other film types.

---

## FAQ

### Q: Do I need to keep the Terminal open?

**A:** Yes, while using the app. Close it when done, restart when needed.

---

### Q: Can I use this on multiple computers?

**A:** Yes! Copy the HTML file with your API key to any computer. Just remember to run a local server.

---

### Q: Will this work offline?

**A:** Timer works offline. AI features need internet connection.

---

### Q: Can I use someone else's API key?

**A:** No, each person should have their own. API keys are personal and track usage to your account.

---

### Q: How do I stop the server?

**A:** In Terminal, press `Control + C` (Mac/Windows/Linux)

---

### Q: What if I lose my API key?

**A:** You can't recover it, but you can create a new one in the Anthropic console. Delete the old one for security.

---

### Q: Is there a mobile version?

**A:** The app works on mobile browsers, but is optimized for desktop/laptop. Mobile version could be improved!

---

## Getting Help

**If you're stuck:**

1. **Check this guide again** - carefully follow each step
2. **Read error messages** - they usually tell you what's wrong  
3. **Check Anthropic docs** - https://docs.anthropic.com
4. **Search online** - "Anthropic API [your error]"
5. **Open an issue** - https://github.com/YOUR-USERNAME/film-dev-coach/issues

---

## Summary: Quick Reference
```
1. Get API key from console.anthropic.com
2. Edit HTML file:
   - Replace YOUR_API_KEY_HERE with your key
   - Change false to true
3. Run local server:
   - python3 -m http.server 8000
4. Open localhost:8000 in browser
5. Click AI Assistant button
6. Ask questions!
```

---

**That's it!** The AI assistant should now work. Enjoy developing your film! 🎞️

---

*Having trouble? Open an issue on GitHub and I'll help troubleshoot!*
