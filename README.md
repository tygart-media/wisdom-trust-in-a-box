# wisdom-trust-in-a-box

An open-source starter kit for capturing elders' life stories at retirement facilities — and leaving them behind like a fortune.

**The idea:** every retirement facility should be a library. A hundred residents hold centuries of lived knowledge. This kit gives any facility (or any builder) everything needed to pilot a life-story capture program: the questions, the onboarding flow, the consent language, the pitch, and the economics.

**Status:** seed. Made to be picked up, forked, and improved by gardeners. MIT licensed — take it.

## What's in the box

```
wisdom-trust-in-a-box/
├── README.md                        ← you are here
├── prompts/
│   └── question-library.md          ← 40 prompts across a life
├── onboarding/
│   └── one-link-flow.md             ← email → link → chat; staff script + family email
├── legal/
│   └── consent-template.md          ← plain-language consent; the elder owns their stories
├── pilot/
│   ├── facility-one-pager.md        ← the 3-minute brief for a facility director
│   └── why-facilities-want-this.md  ← the economics, one page
├── automation/                      ← THE MACHINE HALF: plug an AI in here
│   ├── README.md                    ← the plug-in contract (AI starts here)
│   ├── pipeline.md                  ← 8 stages: raw audio → curated collection
│   ├── schemas.json                 ← JSON schemas for every bucket
│   ├── collection-template.json     ← the EMPTY buckets (copy per resident)
│   ├── example-filled.json          ← one worked example of "done"
│   └── prompts.md                   ← copy-paste prompts for each stage
```

## The non-negotiables

1. **The elder owns their stories.** Period. Families get access by the elder's grant, not by default. The consent template says this in plain language.
2. **One link, no app, no account.** If the storyteller has to install anything, the design failed. The onboarding flow protects this ruthlessly.
3. **Staff are guides, not interviewers.** The kit's staff script keeps the resident in charge of what they share and when they stop.
4. **Start with ten residents, not a hundred.** The pilot one-pager scopes a 30-day, 10-resident pilot. Prove the joy before you scale the logistics.

## How to pilot it (30 days)

1. Pick one facility and one staff champion (activities director is ideal).
2. Send the family email template to ten families; get consent signed.
3. Run the one-link flow: one emailed question per week, resident answers by typing or talking.
4. At day 30, gather the families, play back highlights, and ask: *was this worth it?*
5. If yes, you have your case study. If no, you learned cheap.

## The automation layer (for builders)

A transcript is not a product. `automation/` is the machine-readable half
of the kit: an 8-stage pipeline spec (ingest → clean → segment → enrich →
illustrate → weekly digest → gallery page → gap-finding), JSON schemas for
every bucket, a per-resident collection template with empty buckets, a
worked example, and copy-paste prompts for each stage. Point any AI agent
at `automation/README.md` and it knows how to turn raw recordings into a
curated collection: cleaned stories, quote cards, a life timeline, a
people/places index, generated artifacts (image, music, video, or voice —
each labeled with what it is, what it is NOT, and why it was made), a
weekly family letter, and a gallery page draft — "a life
shown back to the family." The pipeline also notices what's missing and
suggests the next questions to ask.

## Why this works now

The AI was never the hard part. The hard part was onboarding an 85-year-old — and the email-prompt → one-link → chat pattern finally cracked it. What used to be the ceiling is now the floor.

## License

MIT. This is a seed, not a product. If you grow it into something, a credit line is appreciated but not required.

---

*Seeded by Will Tygart / Glint, September 2026. Idea credit: the "wisdom trust" frame — a life documented like a 401k left behind.*
