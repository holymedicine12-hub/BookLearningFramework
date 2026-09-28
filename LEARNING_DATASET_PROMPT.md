# BookLearningFramework — Universal Chunk Generation Prompt

Generate reading-learning chunks for any book in this repository.

## Canonical rules
- Read the canonical source assigned to the current book.
- Process the entire source.
- Preserve source English exactly in `sentences[].en`; never rewrite, simplify, modernize, correct, normalize, or paraphrase it.
- Every source sentence must be covered exactly once across the book.
- One chunk = ONE JSON FILE under `books/{bookId}/chunks/`. Never merge chunks.
- Use the Universal Chunk Schema v2.0.0.
- Required top-level fields: `bookId`, `id`, `examType`, `title`, `thematicTopicId`, `difficultyLevelId`, `sourceSectionId`, `sourceSectionTitle`, `sentences`, `glossary`, `questions`.

## Languages
Supported: `en`, `ur`, `fa`, `ar`.
Every sentence must have all four languages; no empty fields.
English is the canonical source. Urdu and Persian are faithful translations. Arabic uses full tashkeel; preserve Qur'anic Arabic in ﴿ ﴾ and hadith quotations in « » when applicable; otherwise use Modern Standard Arabic.

## Sentences
Each sentence object is exactly:
```json
{"en":"Exact source sentence.","ur":"Urdu translation.","fa":"Persian translation.","ar":"Arabic translation with full tashkeel."}
```
Do not add extra sentence fields unless the active schema explicitly requires them.

## Glossary
Each chunk: 8–30 entries. Each entry:
```json
{"en":"word or phrase","ur":"Urdu equivalent","fa":"Persian equivalent","ar":"Arabic equivalent with full tashkeel","categoryId":"gloss_noun"}
```
Valid categories: `gloss_noun`, `gloss_verb`, `gloss_proper_noun`, `gloss_quranic_term`, `gloss_religious_term`, `gloss_adjective`, `gloss_adverb`, `gloss_phrase`.

## Questions
Each chunk: 3–8 questions. Required fields:
`id`, `type`, `typeId`, `question`, `answer`, `explanationEn`, `explanationUr`, `explanationFa`, `explanationAr`.
Valid typeIds: `q_short`, `q_mcq`, `q_true_false`, `q_fill_blank`, `q_long`.
For MCQ also provide `choices`, `choiceIds`, and `answerId`; `answer` must exactly match the correct choice.

## Book-agnostic metadata
Never hard-code a particular book into this prompt.
- `bookId` resolves to `books/{bookId}/manifest.json`.
- `thematicTopicId` resolves to that book's `thematicTopics`.
- `sourceSectionId` resolves to that book's `sections`.
- `sourceSectionTitle` preserves the source heading.
- `examType` must be a global supported exam type.
- `difficultyLevelId` must be `diff_easy`, `diff_medium`, or `diff_hard`.

## Generic required output example
```json
{
  "bookId": "example-book",
  "id": "example-book-s1-topic-01",
  "examType": "reading-comprehension",
  "title": "Example Reading Topic",
  "thematicTopicId": "topic_example",
  "difficultyLevelId": "diff_medium",
  "sourceSectionId": "section_1",
  "sourceSectionTitle": "Original Section Heading",
  "sentences": [
    {"en":"Exact source sentence.","ur":"اسی جملے کا اردو ترجمہ۔","fa":"ترجمهٔ فارسی همان جمله.","ar":"تَرْجَمَةُ الْجُمْلَةِ بِالتَّشْكِيلِ."},
    {"en":"A second exact source sentence.","ur":"دوسرے جملے کا اردو ترجمہ۔","fa":"ترجمهٔ فارسی جملهٔ دوم.","ar":"تَرْجَمَةُ الْجُمْلَةِ الثَّانِيَةِ بِالتَّشْكِيلِ."},
    {"en":"A third exact source sentence.","ur":"تیسرے جملے کا اردو ترجمہ۔","fa":"ترجمهٔ فارسی جملهٔ سوم.","ar":"تَرْجَمَةُ الْجُمْلَةِ الثَّالِثَةِ بِالتَّشْكِيلِ."},
    {"en":"A fourth exact source sentence.","ur":"چوتھے جملے کا اردو ترجمہ۔","fa":"ترجمهٔ فارسی جملهٔ چهارم.","ar":"تَرْجَمَةُ الْجُمْلَةِ الرَّابِعَةِ بِالتَّشْكِيلِ."}
  ],
  "glossary": [
    {"en":"example","ur":"مثال","fa":"نمونه","ar":"مِثَال","categoryId":"gloss_noun"},
    {"en":"preserve","ur":"محفوظ رکھنا","fa":"حفظ کردن","ar":"يَحْفَظُ","categoryId":"gloss_verb"}
  ],
  "questions": [
    {
      "id":"example-book-s1-topic-01-q1",
      "type":"short",
      "typeId":"q_short",
      "question":"What is the main point of the passage?",
      "answer":"The passage introduces the main topic.",
      "explanationEn":"The passage directly introduces its central topic.",
      "explanationUr":"متن براہِ راست اپنے مرکزی موضوع کا تعارف کراتا ہے۔",
      "explanationFa":"متن مستقیماً موضوع اصلی خود را معرفی می‌کند.",
      "explanationAr":"يُقَدِّمُ النَّصُّ مُبَاشَرَةً مَوْضُوعَهُ الرَّئِيسِيَّ."
    },
    {
      "id":"example-book-s1-topic-01-q2",
      "type":"mcq",
      "typeId":"q_mcq",
      "question":"Which statement matches the passage?",
      "choices":["The passage introduces the topic.","The passage discusses an unrelated topic.","The passage contains no main idea.","The passage is only a glossary."],
      "choiceIds":["example-book-s1-topic-01-q2-c1","example-book-s1-topic-01-q2-c2","example-book-s1-topic-01-q2-c3","example-book-s1-topic-01-q2-c4"],
      "answer":"The passage introduces the topic.",
      "answerId":"example-book-s1-topic-01-q2-c1",
      "explanationEn":"The passage explicitly introduces the topic.",
      "explanationUr":"متن واضح طور پر موضوع کا تعارف کراتا ہے۔",
      "explanationFa":"متن به‌روشنی موضوع را معرفی می‌کند.",
      "explanationAr":"يُقَدِّمُ النَّصُّ الْمَوْضُوعَ بِوُضُوحٍ."
    },
    {
      "id":"example-book-s1-topic-01-q3",
      "type":"true_false",
      "typeId":"q_true_false",
      "question":"The passage has a central topic.",
      "answer":"True",
      "explanationEn":"The passage is organized around a central topic.",
      "explanationUr":"متن ایک مرکزی موضوع کے گرد منظم ہے۔",
      "explanationFa":"متن حول یک موضوع اصلی سازمان یافته است.",
      "explanationAr":"يَدُورُ النَّصُّ حَوْلَ مَوْضُوعٍ رَئِيسِيٍّ."
    }
  ]
}
```

## Validation
Before saving any chunk:
1. Valid JSON.
2. All required top-level fields present.
3. 4–30 sentences.
4. Every sentence has non-empty `en`, `ur`, `fa`, `ar`.
5. 8–30 glossary entries with valid category IDs.
6. 3–8 questions with all required explanations.
7. MCQ arrays and answer IDs are consistent.
8. Every source sentence appears in exactly one chunk.
9. No source text is omitted, duplicated, or silently changed.
10. Chunk IDs and question IDs are unique and stable.

Do not replace this schema with a custom structure. Do not merge chunks. Do not omit translations.
