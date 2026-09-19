# The Pipeline — raw audio → curated collection

Eight stages. Each stage has one input, one output, and one prompt
(see `prompts.md`). Stages 1–3 run automatically on every new recording.
Stages 4–8 run on demand or on a schedule (weekly digest, gallery refresh).

```
recording
  │  1. INGEST
  ▼
transcript (raw)
  │  2. CLEAN
  ▼
transcript (clean)
  │  3. SEGMENT
  ▼
stories[] ──────────────────────────────┐
  │  4. ENRICH                          │  each story fills buckets
  ▼                                     ▼
timeline events[]   quote cards[]   people/places index
  │  5. RENDER
  ▼
artifacts[] → image / music / video / voice (each labeled: what it is,
              what it is NOT, and why it was made)
  │  6. DIGEST (weekly)
  ▼
family letter (draft for review)
  │  7. GATHER (on demand)
  ▼
gallery page (draft for review)
  │  8. NOTICE GAPS
  ▼
suggested next questions → back to the question library
```

## Stage specs

### 1. INGEST
- **Input:** audio file (or typed text) from the one-link flow.
- **Output:** raw transcript with timestamps, saved to
  `collections/<resident-id>/raw/<date>-<question-slug>.txt`.
- **Prompt:** none — transcription only. Keep every word, including
  mistakes; cleaning is stage 2's job.

### 2. CLEAN
- **Input:** raw transcript.
- **Output:** clean transcript — ums, false starts, and repetitions
  removed; nothing added, nothing reworded.
- **Prompt:** `prompts.md → 02-clean`.
- **Bucket filled:** `stories[].clean_transcript` (after segmentation).

### 3. SEGMENT
- **Input:** clean transcript + the question that prompted it.
- **Output:** one or more story records. A single answer often
  contains several stories — split them, don't merge them.
- **Prompt:** `prompts.md → 03-segment`.
- **Bucket filled:** `stories[]` (see `schemas.json`).

### 4. ENRICH
- **Input:** a story record.
- **Output:** timeline events, quote cards, and people/place entities
  extracted from the story.
- **Prompts:** `prompts.md → 04-timeline`, `05-quotes`, `06-entities`.
- **Buckets filled:** `timeline[]`, `quote_cards[]`, `people[]`, `places[]`.

### 5. RENDER
- **Input:** a story record.
- **Output:** 1–3 generated artifacts — an image, a piece of music, a
  short video, or a voice reading — each produced from a generation
  prompt stored with the artifact.
- **Prompt:** `prompts.md → 07-render`.
- **Bucket filled:** `artifacts[]` (prompt + artifact + label + why).
- **The rule:** every artifact explains itself — what it is, what it is
  NOT, and why it was made. A song generated from a story about a
  mother's kitchen is labeled as a song *inspired by* the story, with a
  sentence on the thinking behind it. Abstraction is allowed; deception
  is not.

### 6. DIGEST (weekly)
- **Input:** this week's new stories.
- **Output:** a short family letter — what was recorded, one best
  quote, what's coming next week. **Draft for review**, never auto-sent.
- **Prompt:** `prompts.md → 08-digest`.
- **Bucket filled:** `digests[]`.

### 7. GATHER (on demand)
- **Input:** the full collection.
- **Output:** a gallery page draft — stories arranged as a walkable
  timeline with quote cards, illustrations, and audio clips.
  Think: *a life shown back to the family.* **Draft for review.**
- **Prompt:** `prompts.md → 09-gather`.
- **Bucket filled:** `gallery` (a render spec, not the rendered page —
  the implementer chooses the renderer).

### 8. NOTICE GAPS
- **Input:** the full collection.
- **Output:** 3–5 suggested next questions targeting thin areas
  (an empty decade, a named person with no story, a place mentioned
  three times but never described).
- **Prompt:** `prompts.md → 10-gaps`.
- **Bucket filled:** `suggested_questions[]` — fed back to the
  archivist for upcoming weeks.

## Implementation notes

- **Storage is the implementer's choice.** The schemas describe *shape*,
  not technology — JSON files, a database, Notion, whatever fits.
  The template uses plain JSON files because every AI can read them.
- **Idempotency:** re-running a stage on the same input must not
  duplicate buckets. Key stories by `id`; upsert, don't append.
- **The resident can always see the machine's work.** Every AI-generated
  artifact links back to the source story and timestamp. Nothing is
  a black box to the person whose life it is.
