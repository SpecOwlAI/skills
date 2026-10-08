---
name: refine-ticket
description: Refine a SpecOwl ticket toward Ready by closing the gaps its readiness check found, with parts, questions, link suggestions and clearer rules, then ask the user to run the check again. Use when the user asks to refine a ticket by its id, such as CF-14, asks what keeps it from Ready, or asks to get a Business board ticket into the sprint.
---

Close the gaps that keep a ticket from Ready, so the next readiness check scores it higher. The user keeps the checks, which cost money, and the decisions: you write what the docs and code state, ask people for what they don't, and suggest the rest. Let the team see where you are: the ticket's card and page show you working and the rule you are on.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill refine-ticket and the ticket id. From then on, report each stage as you reach it with report_stage: "Reading the brief", then "Refining BR-n" for each rule you work on, then "Summing up". Don't mark rules building or built; that is for implementing.
2. Brief first. Report "Reading the brief" and read the ticket with get_ticket_brief. Its status line gives the readiness % and the weakest rule, and says "stale" when the ticket or a linked section changed after the latest check; each rule's heading is followed by its readiness % and why line; its Readiness section lists the failing structure checks. For docs beyond the brief, follow the find-docs skill.
3. Check first. When the brief says "readiness not checked", or the check is stale, ask the user to run the check on the ticket (Run check on its page) and stop: change nothing until they ask again after the check. If they say to go on anyway, work from what the brief shows ("not stated" parts, links, done-whens and structure checks) in rule order, and say that this order may not match the real weakest rule.
4. Column. The brief's Columns lists the project's columns, the one the ticket is in and the moves from it, each with its rules; a ticket moves only along a move the brief lists. A column counts as a stage: a built-in one as itself, one of the project's own as the brief says ("counts as Ready"). On a ticket that counts as Idea, offer the move the brief lists to a column counting as Refining (Business), and move it with move_status only when the user says yes; on no, refine it anyway and leave it where it is. On a ticket that counts as Business ready or Sprint ready, refine it without offering a move: those moves are business's. A request to get the ticket into the sprint is the exception to both: step 11 says how. On a ticket that counts as Ready or any later stage (In progress, In review or Done), name its column, say your changes will make its check stale (and, for a column that counts as Ready, that it may have to move back to Refining (Dev) with the reason "clarification"), and change nothing until the user agrees; nothing at all if they don't. Agreeing doesn't move it back: a move stays the user's own request. When steps 3 and 4 both apply, ask both in one message.
5. Order. Close every gap on the weakest rule first, each one closed or turned into a question, before you touch another rule. Then go through the other rules in rule order (BR-1, BR-2, …), skipping the weakest. A rule with nothing you can close is passed over and named in your summary.
6. Each gap with the tool that fits it. A gap is a reason in the rule's why line, a part marked "not stated", or a failing structure check on it:
   - A part the docs or the code state: set_rule_part with what they say.
   - A part nobody states: add_question on that rule and part, in the question group whose people can answer it (the brief lists the team's groups). You have a reasonable guess: severity default, with the guess as its suggested default. You have none: severity needs. Blocker only when the rule can't be built without the answer.
   - Edge cases unclear: write the exceptions part when the docs or code state them, else ask about them.
   - A done-when a tester can't check by observing it: rewrite it with update_rule, saying what they do and what they see.
   - No linked section, or a weak link: find the sections as the find-docs skill says, then suggest_link each one you read in full to the rule with the reason it fits. A section the brief already links, or lists under Pending suggestions for that rule, isn't suggested again.
   - Vague words, more than one sentence, or a duplicate rule: reword the rule with update_rule, or split it with add_rule (step 7).
   - An open question a doc section or the code settles: answer it with answer_question (accept_default for a safe default), citing that source: source_section for a section you read in full, source_code as path:lines. When the user gave the answer in chat, quote their words in user_words instead. When you aren't sure, suggest_answer with its source and leave it to people. A part with an open question on it can't be set until the question is closed. Before you add a question to a ticket that already has open ones, triage those as the triage-questions skill says.
   Never write a guess as a part, and never answer a question without a source or the user's words. A question you added yourself in this pass is answered only with a source you didn't have when you asked it; one you added with a guessed default stays open for people. An open blocker you can't settle stays for people; name it in your summary.
7. Rule changes. You may reword, split or add a rule without asking first; tell the user each change: the rule's number, its old sentence, its new sentence or sentences, and the reason (such as "two outcomes in one rule"). A new sentence resets the rule's parts, so set again the parts you can and ask about the rest. Remove a rule with remove_rule only when the user asks.
8. One pass. Go over every rule once, adding every question the ticket needs with no cap, then stop after your summary: the next pass waits for people's answers, a new check and a new request. Never ask a question that is still open again. When a question was answered since the last pass, write its answer into the part it settles with set_rule_part.
9. Summary. Report "Summing up" and end with one message listing, by rule, what you did: parts set, questions added (with group and severity), links suggested, answers suggested, rules and done-whens changed (old and new wording); then what you left: open blockers and rules you couldn't improve. End it with "Run the check on CF-n to see the new score", naming the ticket. Only the user runs a check. When the pass changed nothing, say so and name what people must answer first instead of asking for a check.
10. Ready only when asked. Move the ticket to Ready with move_status only when the user asks you to, only along the moves the brief lists, one at a time. Ready needs what the brief's Columns lists before Ready: a readiness check that isn't stale, readiness at or above the project's threshold, and no open blockers. If the move is refused, quote the refusal word for word and leave the ticket where it is; don't retry it, move the ticket anywhere else or change anything just to pass the gate, unless the user asks again.
11. Into the sprint, only when asked. When the user asks you to get a ticket that counts as a stage before Sprint ready into the sprint ("get CF-12 into the sprint", "make CF-12 sprint ready", "refine CF-12 completely and plan it"), or says yes to that offer from the implement-ticket or write-ticket skill, refine it fully and plan it into a sprint yourself, quoting that one request in user_words on every move. A plain "refine CF-12" is not that request. Go as follows:
   - Business questions. In your pass, ask the user in one message every open question in the team's Business question group that no doc section or the code settles, and record each answer they give with answer_question, quoting their words. Business ready takes the ticket only once every Business question is answered.
   - The check. Your pass makes the ticket's check stale: ask the user to run it, and move nothing until they say it ran. If they say to go on without one, you may refine further, but move nothing.
   - The bar. Plan it only when the brief shows a check that isn't stale, readiness at or above the project's threshold (the brief's Columns names it in Ready's rules) and no open blockers. Otherwise move nothing, and tell the user its readiness %, how many points it is short and its weakest rule.
   - The moves. Move it with move_status one move at a time toward a column that counts as Sprint ready, only along the moves the brief lists: each time its nearest move forward (the listed move to the first column after the ticket's in the brief's order), while that leads to a column counting as Sprint ready or a stage before it. Read that move's rules in the brief first, and the brief again after each move. A move the brief marks "asks why" also takes move_status's reason: the one that fits, else other. Before a move whose rule only warns, stop: name the warning and ask the user why it should go past; pass a why only in their words. When no listed move leads on toward Sprint ready, stop and name the moves the brief lists. A ticket that already counts as Sprint ready isn't moved.
   - The sprint. Sprint ready takes only a ticket in a sprint. Before that move, put the ticket in the project's active sprint (list_sprints names it, whatever its dates) with update_ticket, or keep the sprint it is already in, and name the sprint in your message. With no active sprint, stop before Sprint ready and list the planned sprints by name and dates for the user to pick; with no sprint at all, say the project has no sprint to plan it into, and create none.
   - Stop at Sprint ready. Moves on from there, toward Refining (Dev), Ready and In progress, are the user's request, as the implement-ticket skill says.
   A refused move on the way leaves the ticket where it is (see "A refused move" below).

## A refused move

When move_status refuses any move, quote the refusal to the user word for word and stop. Until the user says how to go on, make no other call on the ticket, except get_ticket_brief to explain the refusal, and don't move it again. Only "SpecOwl is busy; try again in a moment." may be retried, once.

## The user's words

Every call that says "Only when the user asked you to." (such as move_status and remove_rule) quotes in user_words the user's words that asked for it; SpecOwl refuses it without them, and the session shows them. answer_question and accept_default take a source instead when one settles the question (step 6).

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Stage (report_stage): up to 80 characters, on one line.
- User's words (user_words): up to 500 characters.
- Ticket (update_ticket): title up to 200 characters, why up to 2000 characters, out of scope up to 2000 characters.
- Rule (update_rule, add_rule): up to 500 characters.
- Rule part (set_rule_part): up to 500 characters.
- Question (add_question): up to 500 characters.
- Link reason (suggest_link): up to 500 characters.
- Suggested answer (suggest_answer): answer up to 2000 characters, source up to 300 characters.
- Answer (answer_question): up to 2000 characters; source_code up to 300 characters.

## Read-only connection

When the write tools aren't offered, the connection is read only. Read the brief and the docs as you would otherwise, but start, report and change nothing. When the ticket has no check or a stale one, ask the user to run the check and list nothing. When it counts as Idea, Ready or a later stage, say which move or question you would have asked. Asked to get it into the sprint, also name the moves and the sprint you would have made. Otherwise your final message lists, by rule, each change you would have made (parts, questions with group and severity, links with reasons, rule changes), each marked as not made, then says a change needs read & write access. When there's nothing to change, say the ticket has no gaps you could close. When the user asks you to make a change anyway, say a change needs read & write access, and make none.

SpecOwl skills version 0.7.0.
