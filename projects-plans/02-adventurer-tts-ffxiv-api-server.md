# 🌐 API SERVER

## 🎯 Goal
- Serve audio
- Register new spoken lines
- Orchestrate the pipeline

## 🗄️ Database (Flexible Model)

### tts_lines
```sql
CREATE TABLE tts_lines (
  hash TEXT PRIMARY KEY,
  text_en TEXT NOT NULL,
  npc TEXT,
  voice TEXT,
  body_type TEXT,
  status VARCHAR(20) DEFAULT 'pending', -- pending, processing, error, done
  error_count INT DEFAULT 0,            -- Dead-letter protection
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP
);
```

### tts_translations
```sql
CREATE TABLE tts_translations (
  hash TEXT,
  language TEXT,
  text TEXT,
  has_audio BOOLEAN DEFAULT FALSE,
  error_count INT DEFAULT 0, -- Dead-letter pattern for TTS errors
  updated_at TIMESTAMP,
  PRIMARY KEY (hash, language),
  FOREIGN KEY (hash) REFERENCES tts_lines(hash)
);
```

## ⚡ Idempotent Insert
```sql
INSERT INTO tts_lines (hash, text_en, npc, voice, body_type)
VALUES (...)
ON CONFLICT (hash) DO NOTHING;
```

## 🌐 API

### GET
```
GET /tts/{hash}?lang=pt_br
```

### POST (Register)
```json
{
  "hash": "abc123",
  "text_en": "...",
  "npc": "...",
  "voice": "...",
  "body_type": "Adult",
  "client_auth": "opaque_token"
}
```

> 🛡️ **Security and Anti-Abuse (Crucial for Solo Dev):**
> - **Strict Validation:** Reject payloads with long `text_en` (>500 chars) or invalid formats.
> - **Lightweight Authentication:** Send an HMAC hash or rotated token to mitigate scripts running outside the game.
> - **Rate Limit in WAF:** Configure Cloudflare (Free Tier) to a max X requests/min per IP.

## 📡 GET Response

### ready
```json
{
  "status": "ready",
  "url": "cdn/audio/pt_br/ab/abc123.opus"
}
```

### pending
```json
{
  "status": "pending"
}
```