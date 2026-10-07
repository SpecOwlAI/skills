---
name: find-docs
description: Find what a SpecOwl project's docs say, read it in full, cite where it comes from, and turn the sections you relied on into link suggestions. Use whenever you need the project's docs, whether the user asks what the docs say about something or another skill (writing, implementing or refining a ticket) needs them.
---

Find what the project's docs say about a question, and say where each part of your answer comes from. search_docs matches words, not meaning: the first hit is often not the best one, and a miss on one phrasing says little. So search several ways before you conclude anything, read what you rely on in full, and leave the sections you used as link suggestions a person can approve.

## Steps

1. Search several ways. When you run this skill on its own, not as a step of another skill, first tell SpecOwl you are starting: start_skill with skill find-docs. Run search_docs once for each of these phrasings:
   - the key nouns of the rule or question ("invite expiry");
   - the ticket's own words and their synonyms, from its title and why ("invitation", "pending member");
   - a heading-style phrasing, the way a spec would title the section ("Invites › Expiry", "Edge cases").
   A query of only common words ("how does it work") answers "No sections match." without searching: rephrase it with the words that matter. Search again with any term a hit teaches you.
2. Conclude only after all of them miss. Say the docs don't cover it only when every phrasing came back with "No sections match." or with sections you read and rejected. Then list the queries you ran, each with its result: "No sections match." or the headings you rejected.
3. Read in full before you rely on it. A snippet shows where a section matched, not what it says. Read each section you rely on with get_doc_section, unless the brief already shows it in full (a linked section that isn't gone, or shortened to fit the brief). Base nothing on a snippet or on memory.
4. Cite each claim. After every statement you base on the docs, give its file and heading path, such as "specs/S6-links.md › Edge cases", and its web link when get_doc_section gives one. When sections disagree, quote both with their citations and say so; don't pick one silently.
5. Suggest the sections you relied on. When you are working on a ticket, suggest_link each section you relied on to the rule it helps (leave the rule out for the whole ticket), with a one-sentence reason naming what the section settles for that rule: "Settles that a relink to the section already linked is refused." When the brief already links the section to that rule or lists it under Pending suggestions for it, skip suggest_link and mention the existing link or suggestion instead. Call link_section only when the user asks you to link it.
6. Follow up gone sections. A brief marks a section that left the docs "(gone from the docs)", sometimes with "Jev's likely replacement". Never quote or rely on a gone section as if it were still true; get_doc_section still returns its last text, marked "(gone)", but use that only to find what replaced it. Search for its successor as in step 1, with the gone section's heading and key terms, and read Jev's likely replacement in full like any other candidate. Then tell the user in chat: the broken link, the successor you found with the reason it covers the same ground, or that you found none and the queries you tried. Call relink (or unlink_section) only when the user says so.

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Link reason (suggest_link): up to 500 characters.
- User's words (user_words): up to 500 characters.

## Read-only connection

When suggest_link isn't offered, the connection is read only. Search, read and cite the same way, and list in your answer the sections you would have suggested, each with its rule and reason.

SpecOwl skills version 0.4.0.
