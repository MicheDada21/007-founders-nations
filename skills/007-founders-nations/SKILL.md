---
name: 007-founders-nations
description: "Finds each founder's country of nationality, birth or upbringing and backs every claim with a verbatim quote in a ledger. Use when asking where the founders of companies or tools come from."
---

# 007 founders nations

The target is each founder's country of origin. Find founder names only as far as you need them to look up the person's country.

Every answer comes from the ledger: one row per claim, with a URL and a verbatim quote read on that page. Memory and search-engine summaries only tell you where to look.

## Definitions

- A founder is a person named as founder or co-founder of the entity, or as creator or original author of the tool itself. Later CEOs, early employees and investors enter only when a source calls them co-founders.
- Country of origin is a nationality, a birth country or the country where the person grew up, recorded as basis `nationality`, `birth` or `raised`. A founder with two countries gets a row for each.
- A team quote states the founders' nationality as a group ("founded by two Swiss engineers"). It supports a country row with `person` set to `team`, and the founders' names become optional for that entity.
- The entity type decides what the answer looks like:

| Type | Founders | Country of origin |
|---|---|---|
| Company or startup | Its founders | Each founder's |
| Product built in-house by a larger company (TypeScript, ChatGPT) | `product of <company>`, plus each documented creator when the user wants them | Each credited creator's, otherwise `n/a` |
| Startup or project later acquired (npm, Affinity) | Its original founders | Each original founder's |
| Open standard or spec (WebAssembly) | `no founders`, with the organizations that started it | `n/a` |
| Open-source project | The original author | The original author's |
| Pseudonymous maker (shadcn, comfyanonymous) | The handle as published | What the maker publishes about themselves, or `not_public` |

## Steps

1. List. Save the entities to `entities.json` as `[{ "name", "url" }]`, keeping any id the user's list carries. When the user wants a sample of a larger list, sort on a hash of each name plus a seed, take the first n, and note the seed so someone else can draw the same sample. Done when the file matches the request.

2. Classify. Give each entity a type from the table, from its URL and its own site. Done when every entity has a type.

3. Founders and countries. Climb the rungs in [references/source-ladder.md](references/source-ladder.md): country rungs first, founder rungs only to name a person the country rungs need. Work in waves and send every lookup of a wave at once; for more than 15 entities, split the waves across subagents when the host can start them; each returns only its claims as ledger rows in JSONL, with no prose, and the main agent alone writes the ledger. Spend at most four lookups per entity and field, plus a fifth in the home-market language when none of the four used it and it isn't English, then record `not_public`. Done when every founder, or the team, has a country status.

4. Ledger. Write `ledger.jsonl`, one row per claim:

   ```json
   {"entity":"Runware","field":"country","person":"Ioana Hreninciuc","value":"Romania","basis":"nationality","url":"https://...","quote":"fondată de românii Flaviu Rădulescu și Ioana Hreninciuc","tier":"press","lang":"ro","retrieved":"2026-11-08","status":"single_source"}
   ```

   | `field` | `value` | `person` | `basis` |
   |---|---|---|---|
   | `founder` | Name, `product of <company>`, `no founders` or `not_public`; an optional `role` names a credited creator's role | none | none |
   | `country` | Country name in English | Founder name or `team` | `nationality`, `birth` or `raised` |

   Carry any id from `entities.json` on every row of that entity.

   `tier`: `primary` for the founder's own words, the entity's own pages and official registries; `press` for bylined journalism, local-language press included; `reference` for Wikipedia and Wikidata together, since Wikidata often copies Wikipedia; `directory` for Crunchbase and similar.

   `status`: `verified` for one primary source or two independent press or reference sources; `single_source` for one press, reference or directory source; `conflict` when sources disagree, one row per version; `not_public` when the budget ran out. Done when every value in the report has a row.

5. Self-check. Reread every row next to its quote. A founder row passes when the quote credits that person as founder, co-founder, creator or original author of this entity. A country row passes when the quote ties the person, or the team, to the country in the way `basis` says. A job, a city, a school or a trip never proves a country. Replace a failing row with a source that passes, or set it to `not_public`. Done when every row passes.

6. Report. One table: entity, type, founders, each founder's countries with their basis, and status. List conflicts under it with both versions, then the ledger path. Done when each cell traces to a ledger row and every `not_public` cell says so in plain words.
