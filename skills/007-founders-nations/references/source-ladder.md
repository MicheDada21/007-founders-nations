# Source ladder

Climb each ladder top down and stop at the first rung that answers the field with a quote.

## Queries

Write a cold query first: the entity name plus `founder` or `founded by`, with no person's name in it. A recalled or guessed name in the query pulls in pages about that name and seems to confirm it whether it is right or not. Add a name only after a cold query or a source page has produced it.

National press often names the founders and their nationality in one sentence ("doi fondatori români", "fondateurs genevois"), so query in the language of the home market as well as in English. Take that language from evidence already on file: the headquarters, a country-code domain, the founder's profile location, the language of earlier sources.

## Country of origin

1. The founder's own words: profile, personal site, interviews and podcasts where they say where they were born, grew up or hold citizenship.
2. National press in the home-market language that names the nationality, team quotes included.
3. Wikipedia biography or infobox, in the founder's language edition too, and Wikidata P27 or P19 when the statement cites a source other than Wikipedia. Wikipedia and Wikidata count as one source together. A Wikidata value with no reference, or only "imported from Wikipedia", is a lead, never a row.
4. A national company registry entry that lists the person's nationality, such as the UK Companies House officers page.

## Founders

1. The entity's own pages, fetched by exact path: `/about`, `/company`, `/team`, `/press`, `/imprint`, `/legal`, and the footer for the legal entity and copyright holder.
2. Maker listings: App Store developer name, GitHub organization and README authors, npm or PyPI package author, Product Hunt makers, the Y Combinator company page.
3. The founder's own profile or site, and interviews where the founder speaks.
4. Bylined press: funding announcements, profiles, local business press.
5. Directories (Crunchbase, TheOrg), as tie-breakers only.

For an in-house product, look for creators in the original launch announcement, the project history and official profiles.

## Reading pages

Use `scripts/passages.ts` in this skill's folder (needs Bun; it uses the Browserbase `browse` CLI when installed and plain HTTP otherwise). Run it by its full path from the folder that holds the ledger. It prints only what the ledger needs, so search results and whole pages stay out of context.

- `bun scripts/passages.ts search "<query>" "<query>" ...` prints one `url  title` line per result. Put every query of a wave in one call.
- `bun scripts/passages.ts read <url> <url> ...` fetches the wave's pages in parallel, keeps each one in `pages/`, and prints about 200 characters around founder and country words. Add the words of the page's language with `--add "fondatori|români"`.
- A tiny page prints its first characters and the command that opens it in a remote browser, whose `browse get markdown` output goes through `bun scripts/passages.ts stdin <url>`.
- Copy each quote from a printed passage. When a passage is cut short, `grep` a few more words around it in the kept page instead of reading the whole file.
- Search tools that return a written summary instead of page text give leads only. Open the page and copy the quote yourself.
- When a page names a legal entity instead of people (`Electric Endeavors LLC`), look up that entity on the next rungs.
- Without Bun, use the host's search and fetch, save the page text to a file, and print only the passages that match with a short script.
