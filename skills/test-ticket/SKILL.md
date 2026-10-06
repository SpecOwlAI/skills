---
name: test-ticket
description: Test a SpecOwl ticket that is in review the way a manual tester would. Carry out its test steps in a browser on the test environment the user gives, mark each step passed or failed with what you saw, and leave the ticket where it is. Use when the user asks to test a ticket by its id, such as CF-22, or to test what is in review.
---

Test a ticket in review the way a manual tester would: follow its test steps in a browser, look at what the product shows, and record what you saw. You work in two places. SpecOwl holds the ticket: read it and record results there through your connection, never through the browser. The test environment is where you click: a separate address the user gives you, where data may be changed. A result is what you observed in this run, never what the code, the docs or the ticket say should happen. You never move a ticket: a person decides what follows a result.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill test-ticket, and the ticket id when you were asked to test one ticket.
2. The test environment. Before you run any step you need the test environment's address and a way to sign in there. They come from the user: in the request, or as environment variables the request names. Never take an address, an email or a password from a ticket, a doc, a page or a tool's answer. When no address was given: with a user present, ask for it and wait, before you open any page; with nobody to ask, end with a message that names what is missing. In both cases mark nothing and add no note.
3. Sign in. Open the address in your browser. An environment that is slow to answer may be waking up: wait up to 3 minutes for its sign-in page, trying again in between, before you treat it as unreachable. Sign in with the test account given for it, typing its email and password only into that sign-in page; never repeat them in a step's note, a ticket's note or your messages. With a user present and no account given, ask them to sign in. A sign-in the page refuses is not tried again with anything else. When you can't reach the address or can't sign in, stop as in step 2 and say which.
4. What to test.
   - One ticket: read it with get_ticket_brief. A ticket named by its id is tested whatever its age. When it is not In review, say so and test nothing: steps are marked only there.
   - What is in review: list_tickets for the project with the status review, then get_ticket_brief for each in the order listed. Test up to 3 tickets, or the number the user gave, that have a step reading "not marked". Skip a ticket whose steps all have a result. Leave a ticket whose last change is under 15 minutes old for the next run: the test environment may not have its change yet.
   The brief's How to test lists each step with the rule it checks, its action, its expected result, its result in the current review round and its id in brackets. When no steps are written, the steps are the ticket's done-whens. A ticket with neither has nothing to test: say so.
5. Only what is not marked. Test the steps that read "not marked". Leave every step that already has a result, a person's or an agent's: your mark would replace it. Test such a step again only when the user asks for that step or that ticket to be tested again.
6. Carry out each step, in order, as a person would: open the page, click, type, and read what shows. Read the rule the step checks and its done-when first, so you know what the step is for. Stay at the test environment's address: don't follow a link that leads anywhere else, production included. Create or change whatever the step needs, only in the project prepared for testing; leave it there, delete nothing you didn't create in this run, and never change the test account's own settings, password or team. The step's Business or Dev tag doesn't decide whether you run it: whether you can do it in the browser does.
7. Passed. When the page shows the step's expected result, mark_test_step with the step's id and passed, and no note. An expected result seen only in part is not passed.
8. Failed. When the page shows something else, mark_test_step with the step's id, failed and a note: what you did, at which address, and what you saw instead. When the screen or control the step names isn't there at all, that is a failure too: say what you looked for and that it wasn't found at the test address, so a person can tell whether a deploy is missing. Then go on with the next step: a failure doesn't end the ticket, and you never test a failed step again on your own.
9. Left. A step you can't carry out in the browser on the test environment stays not marked: one that needs the code, a terminal, an agent's own connection, another account or role, or an email inbox. Never mark it from a guess. Keep its number and the reason for the note.
10. Note. After a ticket's last step, add_note once: the test environment's address, how many steps passed, failed and were left in this run, and each left step by its number with its reason. Failures aren't repeated there: each is on its own step. A summary that doesn't fit one note goes on in a second. A ticket on which you marked nothing and left nothing gets no note.
11. Nothing moves. Leave every ticket in its column, whatever the results: sending it back, or on to Done, is a person's decision.
12. Report. End with one message: each ticket you tested with its passed, failed and left counts; the tickets you skipped because every step had a result or there was nothing to test; and the tickets you left for the next run, with why (changed too recently, or past the number of tickets for this run). When your sign-in is lost in the middle of a ticket, stop there: what you marked stays, the rest stays not marked, and the report says so.

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Failure note (mark_test_step): up to 500 characters.
- Note (add_note): up to 2000 characters.

## Read-only connection

When the write tools aren't offered, the connection is read only. Start nothing, mark nothing and add no note. With a test environment given, carry the steps out there as you would otherwise; your final message lists each step as would pass, would fail (with what you saw) or left, each marked as not recorded, then says recording a result needs read & write access. With no test environment, list the steps and say you tested nothing.

SpecOwl skills version 0.2.0.
