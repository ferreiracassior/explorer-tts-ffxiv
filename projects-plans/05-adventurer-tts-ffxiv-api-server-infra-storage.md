# ☁️ TTS INFRA & STORAGE (AUDIO)

## ☁️ TTS INFRA (GPU CLOUD)
### Strategy
- Batch execution
- On demand
- Shutdown after usage

### Advantages
- Low cost
- Scalable
- Fast (CUDA)

---

## 📦 STORAGE (AUDIO)

### Structure
```text
/audio/{lang}/ab/abc123.opus
```

### 🔥 Hosting (Flexible)
The system was designed to support multiple audio distribution strategies:
- Traditional CDN
- Object Storage (e.g. S3-like)
- Global Cache CDN
- Hybrid Distribution

### 🔐 Security (Optional)
- Signed URLs (token + expiration)
- Rate limiting
- WAF / Anti-bot

### 💡 Observations
- Egress might be free depending on provider/CDN
- Aggressive caching drastically decreases costs
- Files are immutable (hash) → ideal for CDNs

### Recommended Headers
```http
Cache-Control: public, max-age=31536000, immutable
```