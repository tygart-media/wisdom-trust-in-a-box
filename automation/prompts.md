# Stage Prompts — copy-paste, one per pipeline stage

Each prompt is written so any AI — yours, theirs, a facility's — can run
it with the stated input. Keep the elder's voice intact; the machine's
job is curation, not authorship.

---

## 02-clean — clean the transcript

**Input:** raw transcript.
**Output:** clean transcript, same language.

```
Clean this transcript of a spoken life story. Remove filler words (um, uh,
you know), false starts, and repeated phrases. Fix obvious transcription
errors where the meaning is clear. Do NOT reword sentences, improve the
prose, add anything the speaker didn't say, or change the order of what
was said. The speaker's voice and word choice must survive untouched.
Return only the cleaned transcript.
```

## 03-segment — split into stories

**Input:** clean transcript + the question that prompted it.
**Output:** JSON array of story records matching `schemas.json#/story`.

```
This transcript answered the question: "<question>".
Split it into individual STORIES — one memorable unit per story. A single
answer often contains several stories; split them, don't merge them.
For each story produce: id (s-001, s-002...), title (short, human),
summary (2-3 plain sentences), era (free text), themes (a few tags),
clean_transcript (the story's portion, verbatim from the cleaned
transcript), recorded_date, and source (recording ref + question date).
Return JSON only.
```

## 04-timeline — extract life events

**Input:** a story record.
**Output:** JSON array matching `schemas.json#/timeline_event`.

```
From this story, extract every dated or datable life event: jobs started,
moves, marriages, births, losses, firsts. For each: label ("Started at
the drugstore — age 16"), date_text (as the elder said it, e.g. "around
1958"), date_approx (ISO date if you can infer one, else null — NEVER
invent a date), place, and the story_id it came from. If no event is
datable, return an empty array. Return JSON only.
```

## 05-quotes — pull quote cards

**Input:** a story record.
**Output:** JSON array matching `schemas.json#/quote_card`.

```
Pull 1-3 short excerpts from this story that would make someone stop
scrolling — a line of wisdom, a funny detail, a gut-punch. Each quote
must be VERBATIM from the clean transcript: no paraphrasing, no
tightening, no combining sentences. Add one line of context per quote.
Return JSON only.
```

## 06-entities — index people and places

**Input:** a story record.
**Output:** JSON arrays matching `schemas.json#/person` and `#/place`.

```
List every PERSON named or clearly described in this story (with their
relationship to the storyteller) and every PLACE mentioned. Set
needs_story=true for a person/place named 2+ times across the
collection who has never been the SUBJECT of a story — those feed the
gap-finder. Return JSON only.
```

## 07-render — generated artifacts (image, music, video, voice)

**Input:** a story record.
**Output:** JSON array matching `schemas.json#/artifact`.

```
This story could live on as more than words. Propose 1-3 generated
artifacts it could become — an image, a piece of music, a short video,
or a voice reading. For each artifact produce: kind
(image/music/video/voice), generation_prompt (written for the
generator; for images describe a scene in an illustrated/painterly
style, never photorealism, never real people; for music describe mood,
era, and instrumentation; for video describe shots and pacing; for
voice describe the reading style), label (must state what the artifact
IS and what it is NOT — e.g. "Song inspired by '<story title>' — not a
recording of the elder, not a historical document"), and why (2-3
plain sentences: the thinking behind creating this — what in the story
called for it, what it's meant to give the family).
Rules: never claim an artifact depicts the actual event or people; if a
story is too abstract or painful to render tastefully in a given medium,
skip that medium. Return JSON only.
```

## 08-digest — the weekly family letter

**Input:** this week's new stories.
**Output:** a short letter, plain text.

```
Write a warm, short family letter (under 200 words) about this week's
recordings for <resident name>. Say what was recorded, include the
single best verbatim quote, and tease next week's question. Write like
a thoughtful grandchild, not a newsletter. This is a DRAFT — mark it
clearly as needing family review before sending.
```

## 09-gather — the gallery page spec

**Input:** the full collection.
**Output:** JSON matching `schemas.json#/gallery`.

```
Arrange this resident's collection as a walkable gallery — "a life shown
back to the family." Group stories into 3-6 sections with human headings
("Work", "The family table"...), attach the best quote cards and
illustrations to each, order sections to read like a life, not a
database. This is a render SPEC (draft status), not the finished page.
Return JSON only.
```

## 10-gaps — suggest next questions

**Input:** the full collection.
**Output:** 3-5 question strings.

```
Look at this collection and find what's THIN: an empty decade on the
timeline, a person flagged needs_story, a place mentioned three times
but never described, an era with no joy stories. Suggest 3-5 specific
next questions that fill the thinnest gaps — each tied to what you
noticed. Warm, curious tone, in the style of the question library.
Return the questions only, one per line.
```
