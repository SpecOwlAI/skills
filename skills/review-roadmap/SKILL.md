---
name: review-roadmap
description: Review a SpecOwl project's roadmap. Write on each late epic a note that says why it is late, from its tickets and their open questions, and suggest a start and a target date for each undated epic that has tickets. Use when the user asks to review the roadmap, asks why an epic is late or asks for dates for the epics that have none.
---

When an epic is late, someone has to read its tickets to say why, and an epic without dates isn't on the roadmap at all. Do that reading: write on each late epic a note that says why it is late, and suggest dates for each undated epic that has tickets. A note is yours to write; an epic's dates and status are a person's, so you only suggest dates and never change an epic.

## Steps

1. Start. Tell SpecOwl you are starting: start_skill with skill review-roadmap. Then use the project the request or the repo points to (list_projects lists them). Ask only when more than one fits.
2. The roadmap. Read it with get_roadmap. It names the day the forecasts were counted on, then gives one line per epic that has both dates: its dates, its tickets left, its pace and the day it will likely finish with the days against its target date (or the one reason it has no forecast), who is on it, its note and an open date suggestion. Then come the epics missing a date, and a last line when the user's role lacks Manage epics. When it lists no epic, say so and stop.
3. May you write? When get_roadmap says the user's role lacks Manage epics, write nothing in this run, neither a note nor a date suggestion: go on as "Without Manage epics" below says.
4. Which epics are late. An epic is late when its line says it will likely finish days late, or that it is days past the target: its target date has passed with tickets left, with or without a forecast. An epic on target or early isn't late, and neither is one with no forecast whose target date hasn't passed. An epic missing a date is never late. A Done epic isn't on the roadmap and gets no note.
5. Why it is late. For each late epic, read what the tools give and no more:
   - From get_roadmap: the day it was last worked on, and its agents at work, waiting on an answer and idle.
   - list_tickets with the project and the epic: each of its tickets with its column and how many open blockers it has.
   - list_questions for each of its tickets that isn't Done: its open questions.
   The cause is what these show: tickets that wait on an open question (say which question), an agent waiting on an answer, nobody on it since a day, tickets not started yet. With no cause found, give the figures alone, such as "6 tickets left at 1 a week; nothing waits on an answer." Never make up a cause.
6. Write the note. Put what you found on the epic with set_epic_note, in one or two sentences. When the epic's note already says the same, leave it: it keeps its writer. When what you found differs from it, write over it, whoever wrote it, and keep its old text for your report. A refused note is not tried again: keep the tool's sentence for your report and go on with the next epic.
7. Leave the others. An epic that isn't late keeps its note as it is, and one without a note gets none. You clear no note. When an epic that isn't late carries a note saying why it is late, leave it and name it in your report as possibly outdated, so a person can clear it.
8. Dates for the undated. For each epic missing a date that has at least one ticket and no open date suggestion, work out a start and a target date and store them with suggest_epic_dates and your reason. An epic with no tickets gets none. An epic with an open suggestion is left as it is and named in your report.
   - Days a ticket takes, from the project's history, never from a guess: for each dated epic that has a forecast, the days from the day the forecasts were counted on to its likely finish, divided by its tickets left; then the mean over those epics. With no such epic there is no history: suggest nothing, and report the epic as not suggested, saying why.
   - The start: the one the epic has; otherwise the day the forecasts were counted on.
   - The target: the one the epic has, when it is on or after that start; otherwise the later of the start and the day the forecasts were counted on, plus the days its tickets left take. The target is never before the start; the same day is in order.
   - The reason: the figures you counted with, such as "5 tickets left at about 4 days a ticket, the pace of Checkout and Search."
   When the tool answers that nothing was stored because someone ignored those dates, try no other dates for that epic in this run, and report it as not suggested with the tool's sentence; a refused suggestion likewise. A suggestion changes nothing on the epic: someone with Manage epics uses or ignores it on the roadmap.
9. Report. End with one message, taken from what the two writing tools answered:
   - Each epic you wrote a note for, by name, with the note; when it replaced one, say so and whose.
   - Each epic you suggested dates for, by name, with the two dates and the reason; when it replaced an open suggestion, say so.
   - Under its epic, each refused note and each suggestion that stored nothing, with the tool's own sentence.
   - Each open suggestion you left as it was, each undated epic you couldn't suggest for, and each note on an epic that isn't late that may be outdated.
   When you wrote and suggested nothing because there was nothing to write, say that no epic is late and no undated epic has tickets.

## Limits

SpecOwl refuses a text longer than its limit, counted in characters after trimming. Write within it the first time:
- Epic note (set_epic_note): up to 280 characters.
- Reason (suggest_epic_dates): up to 280 characters.

## Without Manage epics

Manage epics is a permission of the user's role in the project, not of your connection, and a note needs it. When get_roadmap says the role lacks it, read as you would otherwise and store nothing, a date suggestion neither. Your report then carries, by epic, the reason you would have written for each late epic and the dates you would have suggested with their reason, each marked as not written, and says that writing them needs the Manage epics permission.

## Read-only connection

When the write tools aren't offered, the connection is read only: a note and a date suggestion would both be refused. Reading the roadmap is open to every connection, so start nothing, read as you would otherwise and give your findings in chat as "Without Manage epics" says, each marked as not written, then say that writing them needs read & write access.

SpecOwl skills version 0.8.0.
