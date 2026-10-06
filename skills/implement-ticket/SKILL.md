---
name: implement-ticket
description: Implement a SpecOwl ticket from its brief, and record on the ticket what you learn from the code. Use when the user asks to implement, build or pick up a ticket by its id, such as CF-12.
---

Implement the ticket against its business rules and done-whens, and leave it better than you found it: a part you had to work out, behaviour the code already has and a doubt nobody settled all go on the ticket, so the next person doesn't repeat your work. Let the team see where you are as you go: the ticket's card and page show you working, the stage you report and how far through your plan you are.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill implement-ticket and the ticket id. From then on, report each stage as you reach it with report_stage, in a few words: "Reading the brief", "Building BR-n", "Writing test steps", "Reporting".
2. Brief first. Report "Reading the brief" and read the ticket with get_ticket_brief before you open any code (the implement_ticket prompt already starts with it). Work from its rules, their parts and done-whens, the linked docs and the answered questions. For docs beyond the brief, or a section it marks gone, follow the find-docs skill.
3. Not Ready? When the brief's status is Idea or Refining, or it lists an open blocker, tell the user what's missing: the status, the blockers, and the readiness % with its weakest rule. With an open blocker, first triage the ticket's open questions as the triage-questions skill says, so the user sees the answers the docs or code already give. Implement and move nothing until they say to go ahead.
4. In progress. Asking you to implement the ticket is asking you to start it: move it into In progress with move_status, once, before you write code, quoting the user's request in user_words ("implement CF-12"), and say so. Read the project's columns and the next column's rules in the brief's Columns first: a ticket moves forward one column at a time. When the next column is In progress, or a column of the project's own that counts as In progress, move it there. When the next column still counts as Ready, move nothing and write no code: name the columns between the ticket and In progress, with their rules, and ask the user; with their go-ahead, move it through them one at a time. Don't move it when it already counts as In progress or later, or when you were only asked to read or review it. From Idea or Refining the move needs a readiness check that isn't stale, readiness at or above the project's threshold, and no open blockers; from Ready on, only the next column's rules hold it. A refused move stops you (see "A refused move" below). Before you write code, pick the branch: when the brief's Code section names a branch to continue on, check it out instead of making a new one; otherwise name a new branch with the ticket's id. Then, still before your first code change, record your plan with set_plan: the steps you expect to take, in the order you will do the work, each a short line in your own words. Make each step one piece of work you finish on its own, so you can mark it done before you start the next. A step that builds a rule starts with its label, such as "BR-3: refuse a move without override", so the ticket page groups it with the other rule steps. Rules you will build in the same edit are one step, not a step each: start it with the first rule's label and name the others at the end, as in "BR-2: refuse the move (with BR-3)". When you write your own implementation plan (such as with superpowers:writing-plans), each of its tasks is one step. Tasks you will finish in the same edit are one step, as rules are. Record the plan again with set_plan only when a step is added, removed, reworded or moved; change a step's state with mark_step only, never by recording the plan again. Before you record a changed plan, mark what you finished with mark_step, since a plan recorded again can't have more done steps than the recorded one; then record it whole, every step with its state, so the steps you finished stay done.
5. Rule by rule. When you start building a rule, mark it with mark_rule as building and report "Building BR-n"; when it is done, mark it built. Rules that share a step are still marked one by one. Marks work only while the ticket counts as In progress. A rule held back by an open question stays building, never built, and your report names the question: SpecOwl refuses a built mark while any question on the rule is open. Every rule you built shows built before you ask to move the ticket on. Work your plan the same way: mark_step started when you begin a step and done when you finish it; several can be started at once. Mark a step done as soon as you finish it, before you start the next one; steps started together, such as by subagents, are each marked done when that step is finished, not when all of them are. Every step is done before you ask to move the ticket on.
6. Parts the brief doesn't state. For each part that reads "not stated":
   - The code settles it: write what the code does with set_rule_part. A part with an open question on it can't be written: answer the question first when its source settles it (step 7), or leave the part for people.
   - It doesn't: add_question on that rule and part, in the question group whose people can answer it. Make it a blocker only when the rule can't be built without the answer.
   Never build a guess. Skip only that part and go on with the other rules; with a blocker, also hold back whatever depends on that rule.
7. Record while you work, not at the end:
   - Behaviour the code already has that a rule changes or relies on: add_code_evidence with the path, the lines, the snippet and a note on what it tells. Fill repo (owner/name from the git remote) and commit (HEAD) whenever git is available.
   - A doubt the brief, the docs and the code don't settle: add_question.
   - An open question a doc section or the code settles: answer it with answer_question (accept_default for a safe default), citing that source: source_section for a section you read in full, source_code as path:lines. When the user gave the answer in chat, quote their words in user_words instead. When you aren't sure, suggest_answer with what you found and leave it to people.
   A question you added yourself in this run is answered only with a source you didn't have when you asked it; one you added with a guessed default stays open for people.
8. Report. Report "Reporting". When you finish, list every done-when, each rule's and the whole ticket's, as met or not met, with where in the code (file and lines). A part you left unbuilt because of a question is not met: name the question. Put the same list on the ticket with add_note; when it doesn't fit in one note, post several in rule order, each with the file and lines of its done-whens.
9. Test steps. Report "Writing test steps" and write how to check your change with set_test_steps, from the rules, their done-whens and the code you changed: each step says what to do and what should happen, tags the one rule it checks and whether Business or Dev runs it, and every rule has at least one step. Someone who didn't build it should be able to follow them. Test steps can be set only while the ticket counts as In progress or In review.
10. Next column. Move the ticket to its next column with move_status (In review, or a column of the project's own after In progress, such as Code review) only when every done-when is met, every rule is marked built, every step of your plan is done and the user agrees. First tell the user which of that column's rules, from the brief, the ticket doesn't meet. Never move it further on your own: any later column is the user's request. Otherwise it stays where it is.
11. Done only when asked. Moving the ticket to Done is the user's request, never your next step. SpecOwl refuses an agent's move to Done without override, even when every test step passed. When the user asks you to move it to Done, first check the brief's How to test: when the latest Review round has steps not passed (from In progress none count as passed), tell them how many and move only if they still say so. Then call move_status with override, quoting their words in user_words; never decide on Done yourself.

## A refused move

When move_status refuses a move, whatever the reason, quote the refusal to the user word for word and stop. Until the user says how to go on, write no code and make no other call on the ticket, except get_ticket_brief to explain the refusal; don't move it again, with override or to another column. Only "SpecOwl is busy; try again in a moment." may be retried, once.

## The user's words

Every call that says "Only when the user asked you to." (such as move_status) quotes in user_words the user's words that asked for it; SpecOwl refuses it without them, and the session shows them. answer_question and accept_default take a source instead when one settles the question (step 7).

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Stage (report_stage): up to 80 characters, on one line.
- User's words (user_words): up to 500 characters.
- Plan (set_plan): 2 to 30 steps, each up to 80 characters.
- Rule part (set_rule_part): up to 500 characters.
- Question (add_question): up to 500 characters.
- Code evidence (add_code_evidence): path up to 300 characters, snippet up to 2000 characters, note up to 500 characters.
- Note (add_note): up to 2000 characters.
- Suggested answer (suggest_answer): answer up to 2000 characters, source up to 300 characters.
- Answer (answer_question): up to 2000 characters; source_code up to 300 characters.
- Test steps (set_test_steps): at most 50 steps, each action and expected result up to 500 characters.

## Read-only connection

When the write tools aren't offered, the connection is read only. Don't start the ticket, report stages, record a plan, mark rules or steps or move the ticket; put the parts, evidence and questions you would have recorded in your final message, next to the report, with each rule's state (built, building or not started) and your plan's steps with theirs (done, started or open).

SpecOwl skills version 0.3.0.
