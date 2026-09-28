# BookLearningFramework

A multi-book repository for structured GCSE-style learning datasets.

## Structure

Each book has its own folder under `books/`.

```
books/
└── <book-slug>/
    ├── manifest.json
    ├── index.json
    ├── gcse.json
    ├── gcse-2.json
    └── README.md
```

## Source preservation

For datasets that require source preservation, `sentences[].en` is the canonical English source layer and must remain verbatim.

## Current book

- `books/peshawar-nights/` — Peshawar Nights
