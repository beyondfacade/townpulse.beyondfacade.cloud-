---
name: eli5
description: Rewrite or summarize content in plain, concise English for a busy executive (think CTO/CPO). Use whenever the user says "eli5" anywhere in a request, or asks for a simple, plain-English, or executive version of an agent response, spec, doc, plan, bug report, or code review comment.
---

# eli5

"eli5" here does not mean explaining to a literal five-year-old. It means explaining to a busy executive, like a CTO or CPO: someone smart who has no time and may not know the technical details.

## When this applies

- The user writes "eli5" anywhere in their request.
- The user asks for a simple, plain-English, shorter, or executive version of something.
- It works on any content: your own responses, specs, docs, plans, bug reports, code review comments, incident writeups.

## Style rules

- Use plain, simple English. Prefer short, common words.
- Be concise. Cut every word that does not add meaning.
- Lead with the point. The first sentence gives the bottom line.
- Lists are single level only. Never nest. Each item is one or two short sentences, also in plain English.
- Avoid jargon. If a technical term is unavoidable, explain it in a few plain words the first time it appears.
- Write complete sentences. Do not compress into fragments, abbreviations, or arrow chains.
- Keep paragraphs short: one to three sentences.
- Use headings only when the content is long enough to need them.
- No emojis.

## What to keep

- The conclusion or decision. That is the whole point.
- Anything the reader must act on: risks, costs, deadlines, open questions.
- Numbers and names that change the reader's decision. Drop the rest.
- Accuracy. Simple must not become wrong. If a simplification loses an important caveat, keep the caveat in one short sentence.

## Output shape

- A short question gets one to three plain sentences. No headers, no list.
- Longer content gets a one-line summary first, then a flat list.
- The whole thing should be readable in under a minute.

## Example

Before:

> The migration failed because the reconciler's idempotency check compares the resource hash against the previously persisted state snapshot, but the snapshot serialization was changed in #4123 to exclude default-valued fields, so hashes no longer match and every resource is treated as drifted, triggering a full re-apply which exceeds the API rate limits.

After:

> The migration failed because of a change we made last month, not bad data. A recent PR changed how we save state, so the system now thinks every resource changed. It tried to re-apply everything at once and hit rate limits. Fix: regenerate the saved state once, then re-run.
