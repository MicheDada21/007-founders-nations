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

- Use a search API that returns result URLs, and a lightweight fetch that returns page text as Markdown. Both are stateless, so run a wave's calls in parallel.
- Extract passages with a short script that prints about 200 characters around matches for `founder|founded|co-founder|created by|©|born|grew up|native|citizen|nationality` and their equivalents in the page's language. Print only those passages, so large pages stay out of context.
- An empty or tiny result means the page renders with JavaScript. Open it in a headless or remote browser.
- Search tools that return a written summary instead of page text give leads only. Open the page and copy the quote yourself.
- When a page names a legal entity instead of people (`Electric Endeavors LLC`), look up that entity on the next rungs.
