# OpenAI Assistant API Chat

**Owner/Maintainer:** [Dharma Kevadiya](https://github.com/Dharma2222)

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FDharma2222%2FOpenAI-Assistant-API-Chat&env=OPENAI_API_KEY&envDescription=OpenAI%20API%20Key&envLink=https%3A%2F%2Fplatform.openai.com%2Faccount%2Fapi-keys&project-name=openai-assistant-api-chat&repository-name=OpenAI-Assistant-API-Chat)

<!-- Optional: add your live demo link when ready -->

<!-- [![Live Demo](https://img.shields.io/badge/Live-Demo-green.svg)](https://your-demo-url.vercel.app) -->

## Introduction

Welcome! This project is a chat application that lets users talk with an AI assistant using the OpenAI API. The default model is **configurable** (e.g., `gpt-4o`, `gpt-4.1`, etc.) via environment variables. The app supports text chat, file uploads for analysis, and (optionally) image understanding.

> This repo is under active refactor (branch: `Code_refactor`) — \~90% complete.

## Features

* **Configurable Assistant**: Set assistant name/model/description for a custom experience.
* **Interactive Chat**: Real-time, context-aware conversations.
* **File Uploads**: Send files for the assistant to analyze.
* **Vision (optional)**: Send images and get descriptions/analysis (model dependent).
* **Function Calls** *(Coming Soon)*: Trigger actions/APIs based on chat context.
* **Code Execution** *(Coming Soon)*: Run lightweight Python code for analysis.

## Beta Status

This is a **beta / work-in-progress**. Expect frequent updates and occasional rough edges.

---

## Quick Start

### Prerequisites

* **Node.js** (LTS recommended)
* **OpenAI API key**

### Install

```bash
git clone https://github.com/Dharma2222/OpenAI-Assistant-API-Chat.git
cd OpenAI-Assistant-API-Chat
npm install
```

### Environment

Create a `.env` file in the project root:

```env
# Required
OPENAI_API_KEY=your_openai_api_key

# Optional (choose a supported model; examples)
# OPENAI_MODEL=gpt-4o
# OPENAI_MODEL=gpt-4.1

# Optional: if the UI expects a default assistant id (client-side)
# REACT_APP_ASSISTANT_ID=asst_xxxxxxxxxxxxxxxxxxxxx
```

> If you’re using client-side access to any envs, ensure the variable names match your framework conventions (e.g., `REACT_APP_*` for CRA, `VITE_*` for Vite, `NEXT_PUBLIC_*` for Next.js client exposure).

### Run (Dev)

```bash
npm run dev
```

---

## One-Click Deploy (Vercel)

Deploy this repository on Vercel:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FDharma2222%2FOpenAI-Assistant-API-Chat&env=OPENAI_API_KEY&envDescription=OpenAI%20API%20Key&envLink=https%3A%2F%2Fplatform.openai.com%2Faccount%2Fapi-keys&project-name=openai-assistant-api-chat&repository-name=OpenAI-Assistant-API-Chat)

Deploy with both **OpenAI API Key** and a **default Assistant ID**:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FDharma2222%2FOpenAI-Assistant-API-Chat&env=OPENAI_API_KEY,REACT_APP_ASSISTANT_ID&envDescription=OpenAI%20API%20Key,Assistant%20ID&envLink=https%3A%2F%2Fplatform.openai.com%2Faccount%2Fapi-keys&project-name=openai-assistant-api-chat&repository-name=OpenAI-Assistant-API-Chat)

---

## Application Architecture

### High-Level Flow

* **ChatManager (`ChatManager.ts`)**: Central controller for chat state and operations.
* **API Layer (`api.js`)**: Front-end wrapper for calling API routes.
* **Assistant Modules (`assistantModules.ts`)**: Prepare uploads, initialize assistants, create threads.
* **Chat Modules (`chatModules.ts`)**: Submit user messages, fetch assistant replies, update UI state.
* **React UI**: Components like `WelcomeForm`, `InputForm`, `MessageList` connect to `ChatManager`.

### Diagram (conceptual)

```
[React UI] → [ChatManager] → [API Layer] → [/api routes] → [OpenAI API]
         ↘──────────────────────── state/messages ─────────────────────↗
```

---

## Detailed Modules

### `ChatManager.ts`

* **Role**: Singleton managing chat state (messages, status, assistant/thread IDs).
* **Key Methods**:

  * `startAssistant(assistantDetails, file, initialMessage)`
  * `sendMessage(input)`
  * `getChatState()`

```ts
class ChatManager {
  private state: ChatState;
  private static instance: ChatManager | null = null;

  private constructor(setChatMessages: (m: any[]) => void, setStatusMessage: (s: string) => void) {
    this.state = { /* ... */ };
  }

  public static getInstance(setChatMessages: (m: any[]) => void, setStatusMessage: (s: string) => void) {
    if (!this.instance) this.instance = new ChatManager(setChatMessages, setStatusMessage);
    return this.instance;
  }

  async startAssistant(assistantDetails: any, file: File | null, initialMessage: string) { /* ... */ }
  async sendMessage(input: string) { /* ... */ }
  getChatState(): ChatState { return this.state; }
}
```

### `api.js`

* **Purpose**: Clean front-end API entrypoint.
* **Examples**:

  * `uploadImageAndGetDescription(base64Image)`
  * `createAssistant(assistantDetails)`
  * `createThread()`, `runAssistant()`

```js
export const uploadImageAndGetDescription = async (base64Image) => {
  // call server route, which calls OpenAI Vision if enabled
};
```

### `assistantModules.ts`

* **Purpose**: Assistant setup operations.
* **Key Functions**:

  * `prepareUploadFile(file, setStatusMessage)`
  * `initializeAssistant(assistantDetails, fileId)`
  * `createChatThread(initialMessage)`

### `chatModules.ts`

* **Purpose**: Chat message lifecycle.
* **Key Functions**:

  * `submitUserMessage(input, threadId)`
  * `fetchAssistantResponse(runId, threadId)`
  * `updateChatState(prev, next, setChatMessages)`

---

## Front-End

* **Hooks**: `useChatState.ts` to bind UI with `ChatManager`.
* **Components**:

  * **`WelcomeForm`**: collect assistant details / seed message
  * **`InputForm`**: send user messages
  * **`MessageList`**: render conversation

---

## API Routes (`/api/*.ts`)

Define serverless endpoints that interact with the OpenAI API:

* `POST /api/assistant/create`
* `POST /api/thread/create`
* `POST /api/run/start`
* `GET  /api/messages/list`
* `GET  /api/run/status`
* etc.

> Adjust the exact filenames/paths to your framework (Next.js, Express, etc.).

---

## Assets

**Demo/GIF**

```html
<!-- Update the path if your assets folder differs -->
<img src="Public/File_upload.gif" alt="Assistant Demo" width="600px" />
```

---

## Contributing

Contributions are welcome!

1. Open an issue for bugs/ideas.
2. Fork the repo and create a feature branch.
3. Submit a PR with a clear description and screenshots if UI changes.

---

## Roadmap

* [ ] Function calling (tools/actions)
* [ ] Code interpreter (Python sandbox)
* [ ] Multi-file uploads with better previews
* [ ] Improved error boundaries & retries
* [ ] Theme & accessibility polish

---

## License

MIT © [Dharma Kevadiya](https://github.com/Dharma2222)

---

## Acknowledgements

* OpenAI API team & docs



