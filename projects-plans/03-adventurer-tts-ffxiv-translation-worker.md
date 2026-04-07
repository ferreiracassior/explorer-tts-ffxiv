# 🌍 TRANSLATION WORKER

## 🎯 Goal
Generate translations on demand.

## Pipeline
```text
EN → Translate → sanitize → save
```

## Query
```sql
SELECT l.hash, l.text_en
FROM tts_lines l
LEFT JOIN tts_translations t
  ON l.hash = t.hash AND t.language = 'pt_br'
WHERE l.status != 'error'
  AND (t.error_count IS NULL OR t.error_count < 3) -- Avoid processing zombie texts
  AND t.text IS NULL;
```

## Sanitization
```ts
function sanitize(text: string): string {
  return text
    .replace(/[^\w\s.,!?]/g, "")
    .replace(/\s+/g, " ")
    .trim();
}
```

## Upsert
```sql
INSERT INTO tts_translations (hash, language, text)
VALUES (...)
ON CONFLICT (hash, language)
DO UPDATE SET text = EXCLUDED.text;
```