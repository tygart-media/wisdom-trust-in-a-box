# Automation layer — plug your AI in here

This directory is the machine-readable half of the kit. A human reads the
`README.md` at the repo root; **an AI agent starts here.**

## The contract

Point your agent at this file and say: *"follow the pipeline in
`automation/pipeline.md`, fill `automation/collection-template.json`
for each resident, and use the prompts in `automation/prompts.md`
at each stage."*

That's the whole integration. The repo tells the AI what to do,
what "done" looks like, and where every output goes.

## What's in here

```
automation/
├── README.md                  ← you are here (the plug-in contract)
├── pipeline.md                ← the 8 stages: raw audio → curated collection
├── schemas.json               ← JSON schemas for every bucket
├── collection-template.json   ← the EMPTY buckets (copy per resident, fill in)
├── example-filled.json        ← one worked example so the AI sees "done"
└── prompts.md                 ← copy-paste prompts for each pipeline stage
```

## The idea in one paragraph

A transcript is not a product. Raw recordings go in one end; a *curated
collection* comes out the other: cleaned stories, quote cards, a life
timeline, an index of people and places, illustration prompts, a weekly
family letter, and a gallery page the family can walk through — "a life
shown back to them." The pipeline also notices what's *missing* and
suggests the next questions to ask. Empty buckets in, living archive out.

## The honesty rules (non-negotiable for any implementation)

1. **Generated artifacts are always labeled — and always explained.** An AI
   may turn a story into an image, a song, a video, or a voice reading —
   but every artifact carries a label saying what it is, what it is NOT,
   and WHY it was created. "Illustration inspired by Margaret's drugstore
   story — not a photograph. Made because the family wanted something to
   hang beside the quote card." The thinking behind the artifact is part
   of the artifact; future generations get the provenance, not just the
   pixels.
2. **The elder's words stay the elder's words.** Cleaning a transcript
   means removing ums and false starts, not improving the prose or
   putting words in their mouth. Quote cards use verbatim excerpts.
3. **The AI suggests; the family decides.** Anything public-facing
   (a shared gallery page, a printed book) needs explicit sign-off.
   The pipeline produces *drafts for review*, never published artifacts.
4. **Gaps are invitations, not failures.** If the timeline has no
   childhood entries, the pipeline suggests childhood questions —
   it never invents childhood events.
