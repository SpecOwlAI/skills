---
name: plan-epic
description: Plan a SpecOwl epic. Read its description, the project's docs on it and the tickets it has, propose in chat the tickets that are missing, and write them into the epic once the user agrees. Use when the user asks to plan an epic, to find the tickets an epic is missing or to fill an epic with tickets.
---

An epic's forecast only sees the work that has tickets, so an epic with three of its eventual ten tickets looks early. Find the tickets the epic is still missing and propose them in chat, each with a title and its rules. You create nothing before the user says yes; on their yes you write each agreed ticket as the write-ticket skill does and put it in the epic.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill plan-epic. Then use the project the request or the repo points to (list_projects lists them). Ask only when more than one fits.
2. The epic. Read the project's epics with list_epics and find the one the user named, by its name ignoring case. When the project has no epic of that name, or the user named none, list the epics in Idea and In progress by name and ask which one. Plan one epic per run: asked for several, plan the first and say each of the others takes a run of its own.
3. Done? When the epic's status is Done, say that it is Done and takes no tickets, propose nothing and stop; ask nothing about its scope. Its status alone decides, whatever its tickets' statuses: a Done epic with open tickets still takes none, and an epic in Idea or In progress whose tickets are all Done is planned like any other. You never change an epic's status, and no tool does.
4. Its tickets. Call list_tickets with the project and the epic: it then returns only that epic's tickets, all of them. Read each one's rules with get_ticket_brief, a Done ticket's too, so you know what the epic already covers.
5. Docs. Search the docs on the epic as the find-docs skill says: its name, the key terms of its description and of its tickets, their synonyms and a heading-style phrasing, and read each section you rely on in full with get_doc_section. Finding no section on the epic is normal: go on from its description and its tickets alone, and say in your proposal that no docs section was found.
6. No description? When list_epics gives the epic no description, ask the user one question before you propose anything: what the epic should cover, naming what its tickets and the docs suggest so they can confirm it. Ask also when the tickets or the docs already show its scope, then wait for the answer. Use the answer for this run only: no tool writes an epic's description, so suggest that the user adds it to the epic.
7. What is missing. Compare what the description (or the user's answer) and the docs say the epic covers with what its tickets' rules cover. A missing ticket is a part of that scope no ticket of the epic covers. For each one, search list_tickets in the project for its key words: when a ticket outside the epic already covers it, propose no new ticket for it; name that ticket in the proposal and offer to put it in the epic instead.
8. Propose, in chat. One message, and nothing written yet:
   - The epic's name, status and description, and the tickets it has, each with its id, title and column.
   - The tickets you propose, numbered, each with a title, a type, and its rules, each one sentence with its done-when, written as the write-ticket skill's Writing rules say.
   - For each proposed ticket, the docs sections it rests on, each by its file and heading path.
   - The tickets outside the epic you offer to put in it.
   Aim for tickets of about 8 to 12 rules, and for about ten tickets in one proposal; when more is missing, say what is left and leave it for another run. When nothing is missing, say that the epic's tickets cover its description, propose none and stop. Otherwise end by asking which of them to write.
9. Nothing before a yes. Create no ticket, and put no ticket in the epic, until the user says yes in chat. Only what they agreed to is written: a yes to some of the proposed tickets writes those and no others. A change they ask for is made to the proposal in chat, and you ask again. After a no, create nothing and stop. Anything short of a clear yes, such as a question, a comment or "looks good", is no yes yet: ask.
10. Write them. On the user's yes, write each agreed ticket in the order proposed, as the write-ticket skill's steps 5 to 7 say: create_ticket with the project, the title, the type, the why, the out of scope, the rules with their done-whens and the epic; then its parts with set_rule_part and add_question, and its link suggestions with suggest_link. Leave out that skill's search for an existing ticket, its summary and its offer to get the ticket into the sprint: a new ticket starts in Idea and stays there. A ticket outside the epic that the user agreed to put in it goes there with update_ticket and its epic.
   - The epic was marked Done or deleted since your proposal: create_ticket refuses the ticket, naming the epic or saying it isn't one of the project's, and nothing is written. Quote the refusal, write no further ticket and stop.
   - create_ticket refuses a field: nothing was written, so fix it and call again.
   - It fails after the ticket exists: it names the ticket and the rules it made. Tell the user which, and don't retry.
11. Summary. End with one message that lists every ticket you created, each with its id, title and open questions; then the tickets you put in the epic, the agreed ones you didn't write and why, and what is left for another run. Make no offer to get them into the sprint.

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Ticket (create_ticket, update_ticket): title up to 200 characters, why up to 2000 characters, out of scope up to 2000 characters.
- Rule (create_ticket): up to 500 characters.
- Rule part (set_rule_part): up to 500 characters.
- Question (add_question): up to 500 characters.
- Link reason (suggest_link): up to 500 characters.

## Read-only connection

When create_ticket isn't offered, the connection is read only. Start nothing, and read and propose as you would otherwise. After the proposal, say that writing the tickets needs read & write access, and create none, whatever the user answers.

SpecOwl skills version 0.8.0.
