# 📕 Memory Books (A SillyTavern Extension)

A next-generation SillyTavern extension for automatic, structured, and reliable memory creation. Mark scenes in chat, generate JSON-based summaries with AI, and store them as vectorized entries in your lorebooks. Supports group chats, advanced profile management, and bulletproof API/model handling.

---

## 🚦 What’s New (v3.0.1)

-**JSON Architecture**: Moved from tool-based to JSON structured output for higher reliability.
-**Group Chat Support**: Full compatibility with group conversations.
-**Manual Lorebook Mode**: Freedom to choose any lorebook for memories, or use the chat-bound one automatically.
-**Enhanced Profiles**: Comprehensive profile system with lorebook settings (activation, position, order, recursion).
-**Bulletproof API Handling**: Directly injects model/temp settings for generation, avoiding conflicts with other extensions.
-**Advanced Error Handling**: Smart retry logic for transient network/API errors.
-**Flag-based Memory Detection**: Reliable `stmemorybooks: true` flag system for identifying memories.
-**Scene Overlap Detection**: Configurable check to prevent creating memories for overlapping message ranges.


---

## 📋 Prerequisites

- **SillyTavern:** 1.13.1+ (latest recommended)
- **API Access:** OpenAI, Claude, Anthropic, OpenRouter, or other chat completion API.
- **Scene Selection:** Start and end markers (start < end) must be set.

> **Not Supported:** Text generation APIs (Kobold, TextGen, etc.)

## 💡 Recommended Global World Info/Lorebook Activation Settings
(If these settings are not used it is unlikely the memories will be pulled in!)

- **Match Whole Words:** leave unchecked (false)
- **Scan Depth:** higher is better (at least 4)
- **Max Recursion Steps:** 2 (general recommendation, not required)
- **Context %:** 40% (based on a context window of 100,000 tokens) - assumes you don't have super-heavy chat history or bots

---

## 🚀 Getting Started

### 1. **Install & Load**
- Load SillyTavern and select a character or group chat.
- Wait for the chevron buttons (► ◄) to appear on chat messages (may take up to 10 seconds).

![Wait for these buttons](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/startup.png)

### 2. **Mark a Scene**
- Click ► on the first message of your scene.
- Click ◄ on the last message.

![Visual feedback showing scene selection](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/button-start.png)

### 3. **Create a Memory**
- Open the Extensions menu (the magic wand 🪄) and click "Create Memory", or use `/creatememory` slash command.
- Confirm settings (profile, context, API/model) if prompted.
- Wait for AI generation and automatic lorebook entry.

---

## 🧭 Modes of Operation

###**Automatic Mode (Default)**
-**How it works:** Automatically uses the lorebook that is bound to your current chat.
-**Best for:** Simplicity and speed. Most users should start here.
-**To use:** Ensure a lorebook is selected in the "Chat Lorebooks" dropdown for your character or group chat.

![Chat lorebook binding example](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/chatlorebook.png)

###**Manual Lorebook Mode**
-**How it works:**Allows you to select a different lorebook for memories on a per-chat basis, ignoring the main chat-bound lorebook.
-**Best for:**Advanced users who want to direct memories to a specific, separate lorebook.
-**To use:**
  1. Enable "Enable Manual Lorebook Mode" in the extension's settings.
  2. The first time you create a memory in a chat, you will be prompted to choose a lorebook.
  3. This choice is saved for that specific chat until you clear it or switch back to Automatic Mode.


---

## 👥 Group Chat Support

- All features work with group chats.
- Scene markers, memory creation, and lorebook integration are stored in group metadata.
- No special setup required—just select a group chat and use as normal.

---

## 📝 Memory Generation

### **JSON-Only Output**
All prompts and presets **must** instruct the AI to return only valid JSON, e.g.:

```json
{
  "title": "Short scene title",
  "content": "Detailed summary of the scene...",
  "keywords": ["keyword1", "keyword2"]
}
```
**No other text is allowed in the response.**

### **Built-in Presets**
1. **Summary:** Detailed beat-by-beat summaries.
2. **Summarize:** Markdown headers for timeline, beats, interactions, outcome.
3. **Synopsis:** Comprehensive, structured markdown.
4. **Sum Up:** Concise beat summary with timeline.
5. **Minimal:** 1-2 sentence summary.

### **Custom Prompts**
- Create your own, but **must** return valid JSON as above.

---

## 📚 Lorebook Integration

- **Automatic Entry Creation:** New memories are stored as entries with all metadata.
- **Flag-Based Detection:** Only entries with the `stmemorybooks` flag are recognized as memories.
- **Auto-Numbering:** Sequential, zero-padded numbering with multiple supported formats (`[000]`, `(000)`, `{000}`, `#000`).
- **Manual/Automatic Order:** Per-profile insertion order settings.
- **Editor Refresh:** Optionally auto-refreshes the lorebook editor after adding a memory.

> **Existing memories must be converted!**
> Use the [Lorebook Converter](https://github.com/aikohanasaki/SillyTavern-MemoryBooks/blob/main/lorebookconverter.html) to add the `stmemorybooks` flag and required fields.

---

## 👤 Profile Management

- **Profiles:** Each profile includes API, model, temperature, prompt/preset, title format, and lorebook settings.
- **Import/Export:** Share profiles as JSON.
- **Profile Creation:** Use the advanced options popup to save new profiles.
- **Per-Profile Overrides:** Temporarily switch API/model/temp for memory creation, then restore your original settings.

---

## ⚙️ Settings & Configuration

![Main settings panel](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/Main.png)

### **Global Settings**
- **Manual Lorebook Mode:** Enable to select lorebooks per chat.
- **Allow Scene Overlap:** Permit or prevent overlapping memory ranges.
- **Always Use Default Profile:** Skip confirmation popups.
- **Show Notifications:** Toggle toast messages.
- **Refresh Editor:** Auto-refresh lorebook editor after memory creation.
- **Token Warning Threshold:** Set warning level for large scenes (default: 30,000).
- **Default Previous Memories:** Number of prior memories to include as context (0-7).
- **Memory Title Format:** Choose or customize (see below).

![Profile configuration](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/Profile.png)

### **Profile Fields**
- **Name:** Display name.
- **API/Provider:** openai, claude, custom, etc.
- **Model:** Model name (e.g., gpt-4, claude-3-opus).
- **Temperature:** 0.0–2.0.
- **Prompt or Preset:** Custom or built-in.
- **Title Format:** Per-profile template.
- **Activation Mode:** Vectorized, Constant, Normal.
- **Position:** ↑Char, ↓Cha, ↑EM, ↓EM, ↑AN.
- **Order Mode:** Auto/manual.
- **Recursion:** Prevent/delay recursion.

---

## 🏷️ Title Formatting

Customize the titles of your lorebook entries using a powerful template system.

-**Placeholders:**
  - `{{title}}` - The title generated by the AI (e.g., "A Fateful Encounter").
  - `{{scene}}` - The message range (e.g., "Scene 15-23").
  - `{{char}}` - The character's name.
  - `{{user}}` - Your user name.
  - `{{messages}}` - The number of messages in the scene.
  - `{{profile}}` - The name of the profile used for generation.
  - `August 13, 2025` - The current date (YYYY-MM-DD format).
  - `11:08 PM` - The current time (HH:mm:ss format).
-**Auto-numbering:**Use `[0]`, `[00]`, `(0)`, `{0}`, or `#0` for sequential, zero-padded numbering.
-**Custom Formats:** You can create your own formats, but they are subject to strict character filtering to ensure compatibility.


---

## 🧵 Context Memories

- **Include up to 7 previous memories** as context for better continuity.
- **Token estimation** includes context memories for accuracy.

![Memory generation with context](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/context.png)

---

## 🎨 Visual Feedback & Accessibility

- **Button States:**
  - Inactive, active, valid selection, in-scene, processing.

![Complete scene selection showing all visual states](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/example.png)


- **Accessibility:**
  - Keyboard navigation, focus indicators, ARIA attributes, reduced motion, mobile-friendly.

---

## 🛠️ Troubleshooting

- **No lorebook available or selected:**
  - In Manual Mode, select a lorebook when prompted.
  - In Automatic Mode, bind a lorebook to your chat.

- **No scene selected:**
  - Mark both start (►) and end (◄) points.

- **Scene overlaps with existing memory:**
  - Choose a different range, or enable "Allow scene overlap" in settings.

![Scene overlap warning](https://github.com/aikohanasaki/imagehost/blob/main/STMemoryBooks/overlap.png)

- **AI failed to generate valid memory:**
  - Use a model that supports JSON output.
  - Check your prompt and model settings.

- **Token warning threshold exceeded:**
  - Use a smaller scene, or increase the threshold.

- **Missing chevron buttons:**
  - Wait for extension to load, or refresh.

- **Character data not available:**
  - Wait for chat/group to fully load.

---

## 📝 Character Restrictions

- **Allowed in titles:**
  - Alphanumeric, `- . ( ) # [ ] { } : ; ,` and standard emoji.
- **Blocked:**
  - Accents, non-ASCII, quotes, slashes, special symbols, underscores.

For detailed character restrictions, see [Character Restrictions in Titles](charset.md).

---

*Developed with love using Claude Sonnet 4, extensive testing, and community feedback.* 🤖💕
