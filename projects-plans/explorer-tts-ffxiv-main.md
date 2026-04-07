# 📘 Explorer TTS FFXIV – Overview

The **Explorer TTS FFXIV** project is a complete solution to provide real-time Text-to-Speech (TTS) dubbing for Final Fantasy XIV dialogs, translated into multiple languages (starting with PT-BR).

## 🚀 How It Works (High-Level Pipeline)

1. **In-game Capture (Plugin)**: The client-side plugin listens for NPC lines (AddonTalk, etc) and generates a unique "hash" using the original English dialog.
2. **Registration and Query (API)**: The client sends the hash to the API requesting the audio in the desired language (e.g. pt_br). If it doesn't exist, the dialog is queued for processing, and a temporary local audio (fallback) is generated while waiting.
3. **Translation (Worker)**: Background processes asynchronously fetch new lines and translate them into supported languages.
4. **Dubbing / TTS (Worker)**: After translation, a dedicated worker (ideally running on a GPU) retrieves the text, generates the audio, and uploads it to a CDN/Storage.
5. **Global Consumption**: All subsequent players who encounter the same dialog line will instantly download the definitive audio via CDN, saving it to their local cache.

## 🗂️ Detailed Documentation Structure

The detailed technical architecture has been divided into specific modules. Please refer to the following project files to dive deeper into each application component:

- [01-dalamud-plugin.md](./01-dalamud-plugin.md): Handles in-game capture, hooks, hash security, fallback, and CDN/API requests.
- [02-api-server.md](./02-api-server.md): PostgreSQL DB structure (lines and translations), API security, and responses.
- [03-translation-worker.md](./03-translation-worker.md): SQL queries, insertion pipeline, sanitization, and processing of English-to-target translations.
- [04-tts-worker.md](./04-tts-worker.md): Pipeline for converting text to audio, Server updates, file naming identifiers (hash+language), and error flags for fallbacks.
- [05-infra-storage.md](./05-infra-storage.md): Architecture for on-demand audio storage in a GPU Cloud, CDN with aggressive caching, and cost-optimization headers.

