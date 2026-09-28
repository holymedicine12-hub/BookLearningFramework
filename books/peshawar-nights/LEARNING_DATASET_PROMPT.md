# Peshawar Nights — BookLearningFramework Dataset Prompt

Use `books/peshawar-nights/book_reference/peshawar_nights_book.txt` as the canonical English source.

## Rules
- Process the ENTIRE source.
- Target 900–1400 English source words per coherent chunk.
- One chunk = exactly one JSON file under `chunks/`.
- Preserve English exactly; never rewrite, simplify, modernize, correct, normalize, or paraphrase source English.
- Every source sentence must have `en`, `ur`, `fa`, and `ar`.
- Vocabulary must have English, Urdu, Persian, Arabic, category, and English definition.
- Learning summaries/objectives/questions are derived layers.
- Continue until every source word is assigned exactly once.
- Validate no gaps, no duplicates, continuous numbering, valid JSON, and complete translations.

## Required JSON example
```json
{
  "schemaVersion": "3.0",
  "bookId": "peshawar-nights",
  "chunkId": "peshawar-nights-001",
  "sequence": 1,
  "source": {
    "reference": "books/peshawar-nights/book_reference/peshawar_nights_book.txt",
    "english": {"verbatim": true},
    "sentences": [
      {"id":"peshawar-nights-001-s001","en":"EXACT SOURCE ENGLISH SENTENCE","ur":"اردو ترجمہ","fa":"ترجمه فارسی","ar":"الترجمة العربية"}
    ]
  },
  "vocabulary": [
    {"word":"genealogy","ur":"نسب","fa":"نسب‌شناسی","ar":"علم الأنساب","definition_en":"The study or record of family descent and ancestry.","category":"noun"}
  ],
  "learning": {
    "summary":{"en":"...","ur":"...","fa":"...","ar":"..."},
    "objectives":[],
    "questions":[]
  }
}
```
The `en` field is canonical source; all other language fields are derived.