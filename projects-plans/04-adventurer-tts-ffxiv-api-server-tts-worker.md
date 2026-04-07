# ⚙️ TTS WORKER

## 🎯 Goal
Generate audio only when necessary, based on the translated text.

## Dynamic Config
```json
{
  "languages_supported": ["en", "pt_br", "es_es"]
}
```

## 🗣️ Voice Engine and Model
- **Engine:** Coqui TTS
- **Model:** XTTSv2 (high quality)
- **Main Feature:** Voice cloning (zero-shot voice cloning)
- **Quality:** High-quality generation parameters will be used.

## 🎙️ Samples and Character Management
- **Audio Samples:** Only public domain and freely reproducible samples will be used.
- **Mapping (Source/Target):** Each character will have a unique voice assigned via a mapping table, **strictly separated per language**.
- **Consistency (Saved per Language):** The actual voice (sample) utilized during generation is saved within the translation record in the database, ensuring the history of the voice used is independently managed for each language.
- **Automatic Reprocessing:** Should the source/target table signal that a character's voice for that language has changed, and it differs from the one already bound to that generated line, the worker will invalidate previous dubs and **reprocess** the character's audio with the new voice.

## 🏷️ Audio Identification (Hash + Language)
To uniquely identify the generated audio, the key or filename will be composed of the base hash of the line (in English) appended with the target language (e.g. `{hash}_{language}.opus`). Since there will be one audio file per language, this format ensures files in storage and CDNs do not collide and are easily handled.

## Pipeline
```text
text → sanitize → model (XTTSv2 + clone params) → encode → upload ({hash}_{language}.opus) → update db
```

## Query
```sql
SELECT t.hash, t.language, t.text, l.npc, vm.target_voice_sample
FROM tts_translations t
JOIN tts_lines l ON l.hash = t.hash
LEFT JOIN voice_mappings vm ON vm.npc = l.npc AND vm.language = t.language -- Source/Target table divided by language
WHERE t.text IS NOT NULL
  AND (
    t.has_audio = FALSE 
    OR t.used_voice_sample != vm.target_voice_sample -- Reprocessing condition
  )
  AND l.status != 'error'
  AND (t.error_count IS NULL OR t.error_count < 3); -- Avoid critical TTS failures
```

## Update
```sql
UPDATE tts_translations
SET has_audio = TRUE,
    used_voice_sample = :target_voice_sample, -- Saves the chosen voice
    updated_at = NOW()
WHERE hash = :hash AND language = :lang;
```