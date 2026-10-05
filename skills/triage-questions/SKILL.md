---
name: triage-questions
description: Triage the open questions of a SpecOwl ticket or project. Suggest the answers the docs or the code already give, with their source, and point out questions that repeat another or sit in the wrong question group. Use when the user asks to triage or go through open questions, for a ticket by its id such as CF-15 or for a project.
---

Many open questions are already answered somewhere in the docs or the code. Find those answers: answer a question a doc section or the code settles, citing that source, and suggest the rest, so the person a question is for decides. Point out questions that repeat another or sit in a group whose people can't answer them; you dismiss or move a question only when the user tells you to.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill triage-questions, and the ticket id when you triage one ticket.
2. What to triage. Only the open questions of what you were given:
   - One ticket: read it with get_ticket_brief. Its Open questions list each question with its group, severity and rule part, and what the latest readiness check found about it: "Jev: likely the same as" another question, the group and severity Jev suggests, and the chance the docs already answer it. Pending suggestions lists the answers already suggested.
   - A project: call list_questions with the project and page 1, then page 2 and on, until the answer says none are left. Each question comes with its ticket, rule, group and severity. Read the brief of each ticket that has open questions with get_ticket_brief, for Jev's findings and its linked docs.
3. Already suggested. Skip a question that already has a suggested answer (under the brief's Pending suggestions, or "An answer is suggested" in list_questions): a new suggestion would silently replace it. Replace it only when the user asks.
4. Look before you suggest. Blockers first, then needs, then safe defaults, for each question left:
   - Search the docs as the find-docs skill says: several phrasings, every section you rely on read in full with get_doc_section (or as the brief shows it), a gone section never cited as current, and both quoted when sections disagree.
   - Search the code of the checkout you run in, when it is the project's repository: the files and their tests.
   - Jev's "the docs likely answer it" tells you where to look first, not what the answer is: find the section yourself.
5. Answer or suggest, with its source. When a doc section or the code settles a question and you are sure, answer it with answer_question (accept_default for a safe default whose default it confirms) in one or two sentences, citing the source: source_section with the id of a section you read in full, or source_code as path and lines ("server/src/routes/gaps.ts:274-290"). When it points to an answer but you aren't sure, suggest_answer with the answer and its source: a doc section as file and heading path ("specs/S6-links.md › Edge cases"), or code as path and lines. Every answer and suggestion cites at least one. When sections disagree, quote both to the user and answer or suggest nothing. When you find neither, do nothing: the question is "found nothing" in your report, with the queries you ran. This holds for every group, business as well as dev. A question you added yourself is answered only with a source you didn't have when you asked it; one added with a guessed default stays open for people.
6. Duplicates. When two open questions on one ticket ask for the same information (Jev's "likely the same as", or your own reading), name both and propose dismissing the newer one as a duplicate of the older, and why. Only on the user's yes, dismiss_question with reason duplicate, naming the older one as the question it repeats. Two questions on different tickets can't be dismissed as duplicates: name both with their tickets and propose nothing.
7. Wrong group. When a question sits in a group whose people can't answer it (a question about the code in a business group, a product decision in a dev group; Jev's "suggests" gives its pick), name the group it should be in, and the severity when that is wrong too, and why. Only on the user's yes, reclassify_question with the group, the severity or both.
8. Nothing else on your own. Call dismiss_question or reclassify_question only when the user tells you to, quoting their words in user_words. When they give an answer, or tell you to use your suggested one, answer_question with it and their words in user_words.
9. Report. End with one message grouped by ticket, blockers first, then needs, then safe defaults. For each question: the answer you gave or suggested and its source, the duplicate or group change you propose, "already suggested", or "found nothing". Then what waits on the user's word: the dismissals and moves you proposed.

## The user's words

Every call that says "Only when the user asked you to." (such as dismiss_question and reclassify_question) quotes in user_words the user's words that asked for it; SpecOwl refuses it without them, and the session shows them. answer_question and accept_default take a source instead when one settles the question (step 5).

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Suggested answer (suggest_answer): answer up to 2000 characters, source up to 300 characters.
- Answer (answer_question): up to 2000 characters; source_code up to 300 characters.
- User's words (user_words): up to 500 characters.

## When it starts

The user asks for it, for a ticket or a project. implement-ticket starts it on its ticket when an open blocker holds the ticket back, and refine-ticket starts it before adding questions to a ticket that already has open ones. Otherwise, when open questions hold up what you are doing, offer triage and wait for the user.

## Read-only connection

When the write tools aren't offered, the connection is read only. Read and search as you would otherwise, but start and change nothing. Your report lists, by ticket, the answers you would have given or suggested with their sources, the duplicates you would have proposed dismissing and the group changes you would have proposed, each marked as not made, then says a change needs read & write access.

SpecOwl skills version 0.1.0.
