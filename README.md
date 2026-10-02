<div align="center">
  <img src="./public/icons/pokeball.png" width="60" height="60" alt="Poké Context Memory icon"/>
  <img src="./src/assests/POKe%20context%20ball%20LOGO.png" width="400" alt="Poké Context Memory logo"/>

  <h2><em>Gotta capture your AI context!</em></h2>

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Chrome Web Store](https://img.shields.io/badge/Chrome-available-brightgreen)](https://chrome.google.com/webstore)
[![Firefox Add-on](https://img.shields.io/badge/Firefox-coming_soon-orange)](https://addons.mozilla.org)

</div>

---

## About the Project

**Poké Context Ball** is an open-source browser extension that helps you capture conversations from supported AI chat platforms, store them locally, generate summaries, and transfer useful context into another chat.

Modern LLM workflows often involve switching between different conversations, models, and platforms. Important decisions, code, requirements, and reasoning can easily get lost when starting a new chat.

Poké Context Ball approaches this problem like a **Pokédex for your LLM conversations**: capture useful conversation data, keep it available locally, summarize it, and reuse that context when continuing your work.

> **Project status:** 🛠️ Active development
>The core conversation capture and local-storage workflow is currently implemented. Summarization and context-processing features are still under development, while the project continues to evolve toward broader platform support and improved context transfer.
---

## 🎯 Problem

Working with multiple AI assistants can create a common problem:

```text
Conversation A
      │
      │ Important context
      ▼
New conversation / Different LLM
      │
      └── No Context, Start from new
```

Users often have to manually copy:

* Requirements
* Previous decisions
* Important explanations
* Code snippets
* Project details
* Constraints
* Conversation history

This becomes increasingly difficult as conversations become longer and AI-assisted projects become more complex.

### 💡 The Goal

Poké Context Ball aims to make this workflow easier:

```text
LLM Conversation
      │
      ▼
Capture conversation
      │
      ▼
Store locally
      │
      ▼
Generate summary
      │
      ▼
Select saved context
      │
      ▼
Transfer to another supported AI chat
```

---

## ✨ Key Features

### Conversation Capture

* Captures conversations from supported AI chat platforms.
* Supports both API-based conversation retrieval and DOM-based extraction where applicable.
* Automatically watches for new chat messages while a supported conversation is open.
* Provides a manual capture workflow through the extension popup.

### Local Storage

Captured conversations and messages are stored locally in the browser using:

* **IndexedDB**
* **Dexie.js**

Conversations contain information such as their platform, title, timestamps, version, and generated summary.

### Conversation Summarization

The extension can send a captured conversation transcript to the **OpenRouter API** for summarization.

The current summarization implementation uses:

```text
openai/gpt-4o-mini
```

> ⚠️ The summarization engine is currently under development and may change as the project evolves.

### Context Transfer

Saved summaries can be selected from the extension popup and injected into another supported AI chat interface, allowing users to continue a conversation with previously captured context.

---

## 🌐 Supported AI Platforms

The current extension is configured for:

* ChatGPT
* Claude
* Gemini
* DeepSeek

Support may vary depending on the current implementation of each platform's interface and APIs.

Gemini API-based capture is currently not supported and falls back to DOM-based extraction.

Additional platform support, including broader compatibility, may be added as development continues.

---

## 🛠️ Tech Stack

### Core

* **TypeScript**
* **Vite**
* **CRXJS Vite Plugin**
* **Chrome Extensions Manifest V3**

### Storage

* **IndexedDB**
* **Dexie.js**
* `chrome.storage.local` for persisted summary data

### Summarization

* **OpenRouter API**
* **OpenAI GPT-4o-mini**

---

## 📁 Project Structure

```text
Poke-Context-Ball/
│
├── public/
│   └── manifest.json
│
├── src/
│   ├── assets/
│   │
│   ├── background/
│   │   └── service-work.ts
│   │
│   ├── content/
│   │   ├── providers/
│   │   ├── conversation-capture.ts
│   │   ├── detector.ts
│   │   ├── dom-extractor.ts
│   │   ├── inject.ts
│   │   ├── mutation-watcher.ts
│   │   └── scroll-engine.ts
│   │
│   ├── extraction/
│   │   ├── entityExtractor.ts
│   │   ├── pipeline.ts
│   │   ├── prompts.ts
│   │   └── summarizer.ts
│   │
│   ├── injection/
│   │   ├── adpaterInterface.ts
│   │   ├── chatgpt.ts
│   │   ├── claude.ts
│   │   ├── deepseek.ts
│   │   ├── gemini.ts
│   │   └── grok.ts
│   │
│   ├── ML/
│   │   └── modelLoder.ts
│   │
│   ├── shared/
│   │   ├── constants.ts
│   │   ├── messages.ts
│   │   ├── transcript.ts
│   │   ├── types.ts
│   │   └── utils.ts
│   │
│   ├── storage/
│   │   ├── contextStore.ts
│   │   ├── db.ts
│   │   ├── exportImport.ts
│   │   └── vectorIndex.ts
│   │
│   └── ui/
│       ├── popup/
│       │   ├── poke-test.css
│       │   ├── popup.css
│       │   ├── popup.html
│       │   ├── popup.ts
│       │   └── poke-test.html
│       └── poke-test.html
│
├── .env.example
├── CONTRIBUTING.md
├── Struct.md
├── THIRD_PARTY_LICENSES.md
├── Workflow.md
├── package.json
├── package-lock.json
├── tsconfig.json
├── vite.config.ts
└── README.md
```

### Main Components

* **`content/`** — Detects supported chat platforms, captures conversations, observes new messages, and handles DOM-based extraction.
* **`background/`** — Handles communication between extension components, conversation storage, API capture, and summarization.
* **`extraction/`** — Contains transcript processing and summarization-related code.
* **`storage/`** — Contains the Dexie database and storage-related modules.
* **`injection/`** — Contains platform-specific adapters used for transferring context into supported AI chat interfaces.
* **`ui/`** — Contains the extension popup and related UI.
* **`shared/`** — Contains shared types, messages, utilities, and constants.

Some modules related to advanced context processing and vector-based functionality are currently placeholders or under development.

---

## 🚀 Setup

### Prerequisites

Make sure you have:

* Node.js
* npm
* Google Chrome or another Chromium-based browser
* An OpenRouter API key for summarization

Basic knowledge of TypeScript and browser extensions is helpful for development.

### 1. Clone the repository

```bash
git clone https://github.com/NerdSWAYAM/Poke-Context-Ball.git
cd Poke-Context-Ball
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a `.env` file based on `.env.example`.

The summarization functionality requires an OpenRouter API key:

```env
OPENROUTER_API_KEY=your_api_key_here
```

Do **not** commit your API key or other secrets to the repository.

The project also supports an optional OpenRouter URL configuration:

```env
OPENROUTER_URL=your_openrouter_url
```

If no custom URL is provided, the default OpenRouter chat-completions endpoint is used.

### 4. Build the extension

```bash
npm run build
```

The production build is generated in the `dist/` directory.

### Development

You can start the Vite development server with:

```bash
npm run dev
```

---

## 🌐 Load the Extension in Chrome

After building the project:

1. Open `chrome://extensions/`
2. Enable **Developer mode**.
3. Select **Load unpacked**.
4. Select the generated `dist/` directory.
5. Open one of the supported AI chat platforms.
6. Open the extension popup to capture and manage conversation context.

For development and debugging, extension and service-worker logs can be inspected through Chrome's extension developer tools.

---

## 🔄 How It Works

The current conversation workflow can be summarized as:

```text
Supported AI Chat
       │
       ▼
Conversation Capture
       │
       ├── API-based capture
       │
       └── DOM-based fallback
       │
       ▼
Normalized Conversation
       │
       ▼
Dexie / IndexedDB
       │
       ▼
Conversation Transcript
       │
       ▼
OpenRouter Summarization
       │
       ▼
Saved Summary
       │
       ▼
Extension Popup
       │
       ▼
Transfer Context
       │
       ▼
Another Supported AI Chat
```

### Capture

The content script detects the current AI platform and conversation. Depending on the platform, the extension can attempt to retrieve the conversation through an internal API and fall back to DOM extraction when necessary.

### Storage

Captured conversations and individual messages are stored locally using Dexie.js over IndexedDB.

### Summarization

The background service worker retrieves stored messages, formats them chronologically, and sends the resulting transcript to the summarization module.

The summarization module uses OpenRouter to generate a summary.

### Transfer

The extension popup displays saved summaries. A selected summary can then be injected into a supported AI chat interface.

---

## 🧪 Testing

Automated testing is currently an area of development.

Contributors can help improve reliability by:

* Adding unit tests
* Adding integration tests
* Testing conversation capture
* Testing storage behavior
* Testing popup functionality
* Testing summarization
* Testing context transfer
* Testing different Chromium-based browsers
* Finding edge cases in AI chat interfaces
* Reporting and reproducing bugs

There is currently no dedicated `npm test` script in `package.json`.

---

## 🤝 Contributing

Contributions are welcome!

Please read [`CONTRIBUTING.md`](./CONTRIBUTING.md) for information about the contribution workflow.

You do not need to be an expert in every part of the project. Contributors interested in learning while working on an open-source browser extension are welcome.

Useful skills include:

* TypeScript
* Vite
* HTML
* CSS
* Browser extensions
* Chromium-based browser development

---

## 📚 Technical Documentation

Additional technical documentation is available in the repository:

* [`Workflow.md`](./Workflow.md) — implementation workflow and development phases
* [`Struct.md`](./Struct.md) — project structure and architecture documentation
* [`THIRD_PARTY_LICENSES.md`](./THIRD_PARTY_LICENSES.md) — third-party licensing information

---

## 📦 Browser Support

### Current

The extension currently targets Chromium-based browsers and is developed and tested primarily with Google Chrome.

Firefox support is also available as part of the project.

### AI Platform Support

Currently configured AI platforms include:

* ChatGPT
* Claude
* Gemini
* DeepSeek

Platform support may change as the extension and individual AI websites evolve.

### Future

Broader browser and platform compatibility is part of the project's ongoing development.

---

## 🔗 Links

**Repository:**
https://github.com/NerdSWAYAM/Poke-Context-Ball

**Issues:**
https://github.com/NerdSWAYAM/Poke-Context-Ball/issues

---

<div align="center">

### ⚡ Gotta catch your context!

**Poké Context Ball**

*Capture it. Store it. Transfer it.*

</div>

