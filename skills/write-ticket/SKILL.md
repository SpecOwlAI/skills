---
name: write-ticket
description: Write a SpecOwl ticket close to Ready from a request or the conversation, with rules, done-whens, out of scope, parts and questions. Use when the user asks to write, create or file a ticket, or to turn the conversation into one.
---

Write a SpecOwl ticket from the user's request, or from the conversation so far when they ask to turn it into one. Create it straight away and then show the user what you created; don't wait for them to confirm a draft. The one exception is a request too big for one ticket: you propose a split first (step 4).

## Steps

1. Project: tell SpecOwl you are starting with start_skill, skill write-ticket. Then use the project the request or the repo points to (list_projects lists them). Ask only when more than one fits.
2. Existing tickets: search list_tickets for the request's key words. If one may already cover the request, show it and let the user pick it or go on.
3. Docs: search and read them as the find-docs skill says: search_docs on the request's key terms, their synonyms and a heading-style phrasing, and read the sections you rely on with get_doc_section. Use the docs' own terms in the ticket, and quote nothing you didn't read. Finding no relevant section is normal for a new feature: go on, and say so in your summary.
4. Before you create, size the request. These are the only times you wait before creating:
   - Too vague for even one testable rule ("make exports better")? Ask the user the one question that unlocks a rule, then go on.
   - Too big for one ticket: it needs more than 12 rules, or it mixes two features a person would build and test apart? Don't write one oversized ticket. Propose a split in chat, in one message, with nothing written yet: the tickets numbered, each with a title, a type and its rules, each one sentence with its done-when, written as below. Aim for about 8 to 12 rules a ticket and at most about ten tickets; when more is needed, say what is left for another run. End by asking which of them to write.
   - Create nothing until the user says yes in chat, and then only what they agreed to: a yes to some of the tickets writes those and no others. A change they ask for is made to the proposal in chat, and you ask again. Anything short of a clear yes, such as a question or "looks good", is no yes yet: ask. After a no, create nothing and stop.
   - On the yes, write each agreed ticket in the order proposed with steps 5 to 7, then give one summary of them all as step 8 says, each with its id, and end it with step 9's offer for each of them.
5. Create it with create_ticket:
   - Title: short, what changes.
   - Type: feature, bug, change or tech.
   - Description: what the ticket is about and the problem it solves, in one to three sentences. An implementation detail the user insists on ("use Papa Parse for the CSV") goes here as a constraint, never in a rule.
   - Out of scope: the user's words if they gave any; otherwise what a reader could expect that the request leaves out (neighbouring features, other platforms, follow-ups mentioned in passing). If nothing obvious is left out, write "Nothing beyond the request."
   - Rules: each one sentence with its done-when, written as below.
   If create_ticket refuses a field, nothing was written: fix it and call again. If it fails after the ticket exists, it names the ticket and the rules it made: tell the user and don't retry.
6. Parts: read the new ticket with get_ticket_brief for its rule ids and the organisation's question groups. Set each part (trigger, who, data, outcome, exceptions) that the request or the docs state with set_rule_part. For every other part, add_question on that rule and part, in the question group whose people can answer it:
   - You have a reasonable guess: severity default, with the guess as its suggested default.
   - You have none: severity needs.
   - Blocker only when the rule can't be implemented at all without the answer.
   Never write a guess as a part.
7. Links: suggest_link each section you relied on to its rule, or to the whole ticket, with the reason it fits.
8. Summary: show the ticket id, title, type, description, out of scope and the numbered rules with their done-whens. Say which rules have no docs, which questions you added, and whether you wrote the out of scope yourself. Make the changes the user asks for with update_ticket, update_rule and add_rule, writing them the same way.
9. Into the sprint? End the summary by offering to refine the new ticket and plan it into the project's active sprint, naming the sprint (list_sprints); with no active sprint, offer only to refine it and say there is no active sprint to plan it into. On yes, go on as the refine-ticket skill says, with its step 11 for the sprint. Until the user says yes, the ticket stays in the column it starts in.

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Ticket (create_ticket, update_ticket): title up to 200 characters, description up to 2000 characters, out of scope up to 2000 characters.
- Rule (create_ticket, add_rule, update_rule): up to 500 characters.
- Rule part (set_rule_part): up to 500 characters.
- Question (add_question): up to 500 characters.
- Link reason (suggest_link): up to 500 characters.

## Writing rules

- One plain sentence about one behaviour. A rule with "and then", or with two outcomes, is two rules.
- Say what happens, not how it's built: no file names, tables, endpoints or code. A tech ticket may state technical behaviour that a developer or the system can observe.
- Give each rule a done-when a tester can check by observing it: what they do and what they see.
- One ticket, one feature, of about 8 to 12 rules: that size is easiest to build and review. A small request, such as most bugs, needs fewer, so never pad a ticket. Splitting a compound rule in two can take a ticket past 12: then split the ticket too, as step 4 says.

## Examples

- A compound rule, split in two:
  "When a user exports a report, it downloads as CSV and the team owner gets an email."
  → "When a user exports a report, it downloads as a CSV file." Done when: exporting a report with 3 rows downloads a CSV with a header and 3 rows.
  → "When a report is exported, the team owner gets an email naming who exported it." Done when: after an export, the owner has one email with the exporter's name.
- A request too big for one ticket, split before creating:
  "Customers can return an order for store credit, and the shop sends a monthly newsletter." Two features, built and tested apart, with about 9 rules for returns and 6 for the newsletter.
  → Proposed in chat: 1. "Return an order for store credit" (feature, its 9 rules); 2. "Monthly newsletter" (feature, its 6 rules). Created only after the user says which to write.
- An implementation detail, rewritten as behaviour:
  "Add a column to the invites table and set it in the mailer."
  → "The team page shows when each pending invite was sent." Done when: an invite sent today shows "sent today" on the team page.
- A vague done-when, made testable:
  "Exports work well." → Done when: exporting 10,000 rows downloads a CSV with all 10,000 rows within 30 seconds.
- A tech ticket's rule:
  "The server connects to the database through the pooler's transaction mode." Done when: with only the pooler's port reachable, the server starts and answers its health check.

SpecOwl skills version 0.9.0.
