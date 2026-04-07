# 🧩 Dalamud Plugin (Client)

## 🎯 Goal
Capture dialogs in real time, generate an immutable hash (EN base), query the API for audio (by language), and play it using a local fallback.

## 🧠 Main Flow
```text
Capture (UI hooks/memory)
        ↓
Normalization (EN)
        ↓
Hash (EN base)
        ↓
GET /tts/{hash}?lang=pt_br
        ↓
┌───────────────┬───────────────┐
│ ready         │ pending       │
│ (audio URL)   │               │
└──────┬────────┴───────┬───────┘
       │                │
 Download & play   Fallback (Piper)
       │                │
  Local cache      Retry/backoff
```

## 🔍 Data Capture
### Sources
- AddonTalk
- AddonBubble
- AddonCutSceneSelectString

### Stack
- Dalamud
- UI Hooks
- FFXIVClientStructs
- Signature scanning

## 🧹 Normalization (EN)
```ts
function normalize(text: string): string {
  return text
    .trim()
    .toLowerCase()
    .replace(/\s+/g, " ")
    .replace(/[^\w\s]/g, "");
}
```

## 🔑 Hash (immutable)
```ts
hash = SHA1(
  normalizedTextEN + "|" +
  npcName + "|" +
  voice + "|" +
  bodyType
)
```
> ⚠️ Never include translation strings in the hash

## 💾 Local Cache
```text
/tts-cache/{lang}/ab/abc123.opus
```

## 🎤 Voice System
```text
Race + Gender + BodyType
```

## 🎚️ Pitch
```csharp
float pitch = bodyType == "Youth" ? 1.2f : 1.0f;
```

## ⚡ Fallback
- Engine: Piper
- Local execution
- Will be replaced later by the definitive audio

## 🔄 Retry
```text
10s → 30s → 2min → 5min
```