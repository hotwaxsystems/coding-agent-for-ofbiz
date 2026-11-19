---
name: write-screen-outline
description: |
  Write or review a screen outline: the HEMP artifact that says who opens a
  screen, what they want to know, and where the page answers it. Use when
  - Designing a new screen from a business process story.
  - Reviewing a built screen against its outline.
  - A story change adds or moves a question on a screen.
---

## Goal
Every screen answers its users' questions in the order they ask them.
The first question is answered in one look.

## Why it works
A screen feels easy when it answers questions in the order people ask them.
People look at the top and left of a page first. So the first question goes
in the header, and the work goes in the main column.

## Method

1. Read the business process story. List each actor who opens this screen,
   with the story step that shows it.
2. For each actor, write the questions they ask, in their order. Give the
   source of each question: a story step, an actor definition or a rule.
3. When the story is silent, write an open question. Never fill the gap.
4. Choose the page type:

   | Page type | Use for |
   | :--- | :--- |
   | Index page | Find and act on many records |
   | Details page | Review and act on one record, and change it one card at a time. Example: View Person |
   | Create page | Record a new record, one or more steps. Example: Create Person |
   | Scan page | One field, scan, confirm, next. For handhelds. Example: Pick |
   | Desk page | Handheld work done at a computer, by keyboard. Example: Paper desk |
   | Report page | Read what the data shows over a chosen time. Example: Order profile |
   | Dashboard | Counts that say how the work is going, each linked to its list |
   | Items into containers | Place items into boxes, pallets, carts or locations. Example: Pack order |

5. Place each question. On a details page:
   - Header: record id, state, problems, actions. The first question of
     every actor goes here.
   - Main column, about two thirds wide: the work, such as lines and history.
   - Side column, about one third wide: facts about the record, such as who,
     where and dates.
   On an index page:
   - A required constraint, such as warehouse and a state or a date range.
     The list never shows "everything". An empty filter is not allowed to
     mean "all records".
   - The constraint shows on the find form, filled in with its default. The
     user sees it and may change it within its limits.
   - Each default comes from what users asked for in the record, such as
     meeting transcripts. Cite the source.
   - A default sort, named in the outline.
   - Pagination, with the page size named in the outline. The count shows
     the whole match.
   - A search box searches one field the user chooses, shown next to it.
     Never guess the field from the value typed.
   - Offer only fields that are cheap for the database: exact ids and codes.
     No free-text search on names, emails or notes.
   - Filter and sort only on fields a database index can serve. Name each
     filter field, so the design note can name its index.
   On an items into containers page:
   - A "not placed yet" list comes first, under the header. It shrinks as
     the work goes on.
   - One card per container below it. One container is open and marked so.
   - A scan puts the item into the open container.
   - The user may tick items and choose any container, or a new one.
   - Each item shows in one place only. It may move to another container,
     or back to the "not placed yet" list.
   - Closing a container asks for its measure, such as weight, and opens the
     next one when items are left.
   - Complete turns on only when the "not placed yet" list is empty.
   On a create page:
   - One column, about two thirds wide. A phone keeps the same order.
   - Cards in the order people fill them in, taken from the story steps.
   - Required fields carry a star. A note at the top says what the star
     means. Ask only for what the story requires; the rest is optional.
   - A choice of kind, such as a person or a shared sign-in, goes in the
     header. It hides the cards that kind does not use.
   - A list inside the record, such as warehouses with roles, adds rows with
     Add. A row added by mistake can be removed before Save.
   - A dated row starts today, and the date can be changed.
   - One Save for the whole page, at the bottom, with Cancel.
   - An error shows under its field, in plain words. A summary at the top
     lists the errors and links to each field. Nothing is saved until all
     are fixed, and the page keeps what was typed.
   - After Save, the details page of the new record opens with a short
     message.
   - Show only the choices the user may make, such as only their own
     warehouse.
   On a details page, to change the record:
   - Each card has its own Edit. The card turns into a form in place, with
     Save and Cancel. The rest of the page stays as it is.
   - One card at a time. While one card is in edit, Edit on the others is
     off.
   - A list card has Add, which opens a row form at the top of the card.
   - A row is ended, never deleted. End asks for an end date, with today as
     the default. An ended row moves under "Show history" on its card.
   - Edit, Add and End show only where the user may change that row.
   - An error shows under its field, inside the card. After Save, the card
     returns to read mode with a short "Saved" mark.
   - Actions on the whole record, such as turning off a sign-in, go in the
     header. One that changes other work says so in its confirm.
   On a dashboard:
   - Header: the scope, such as warehouse, merchant, and day or time window.
   - Work that needs a person, such as stuck or failed work, comes first,
     right under the header.
   - One row of count cards per question, in the order the actor asks.
   - A total shows once. Its parts follow it and add up to it.
   - The scope applies only to the counts it really limits.
   - All rows are full width, with no side column, so a phone keeps the
     same order.
   - Each card is a number with a label. The whole card opens the list
     behind the count, which starts with the same count.
   - Each actor sees only the cards that answer its own questions.
   - No alerts. The person checks the board.
   On a scan page:
   - One task per screen, such as one line to pick.
   - The header answers "where do I go, and what do I take", in large text.
   - The scan steps sit under the header, one row each, ticked as scanned.
   - The scan field is always focused. Each scan goes to the next step not
     yet ticked.
   - A wrong scan shows red, with what was expected.
   - The next task opens by itself when the last step is ticked. No extra
     tap.
   - Two ways out when a scan cannot be done: confirm by tap, when a
     barcode will not scan, and short, when the item is not there. A tapped
     task is recorded as tapped.
   - The footer shows how much is left, such as "3 of 12", and what comes
     next.
   - Pause goes back to the start. The work stays with the worker.
   - One column, phone width, no menu bar. Large text and buttons, for one
     thumb and gloves.
   - Every label comes from the label file, so a second language can be
     added later.
   On a desk page:
   - One column, at a computer.
   - One tab for each part of the job, such as Start and Finish.
   - Keyboard first. Enter moves to the next field.
   - It records the same facts as the scan page it stands in for.
   On a report page:
   - Header: the scope, the time choice, and the counts that answer
     question 1.
   - Each view is a tab. The tab that answers question 1 opens first.
   - Anything to act on goes in the side column, such as suggestions.
   - The page only reads. Actions in the side column change only what they
     name.
6. Name the data blocks each place uses, such as address or status.
7. If two actors' first questions fight for the header, propose two screens.

## Shape of the outline

```markdown
# <Screen> screen outline

<One line: the record, and who opens it.>

## Actors and activities
| Actor | Activity | Source |

## <Actor> questions
| # | Question | Answered by | Source |

## Page layout
<Page type. Header, main column, side column: what each holds.>

## Open questions
<Only when the story is silent.>

## Review checks
1. Each question is answered in the place this outline names.
2. Question 1 of each actor is answered in one look, with no scrolling.
3. Anything on the page that answers no question moves down, hides, or goes.
```

## Worked example: View Order in a warehouse system

The order requirements say Customer Service fixes held orders, and the
Warehouse Manager follows orders and pulls them back.

| # | Customer Service asks | Warehouse Manager asks |
| :--- | :--- | :--- |
| 1 | Is the order held, and for what? | Where is the order in the warehouse now? |
| 2 | What must I fix? | Is something stopping it? |
| 3 | Can the order still change? | What can I do now? |
| 4 | Whose order is it? | Who has it? |
| 5 | What is in it? | When must it ship? |

Both first questions are about state. So one header serves both: the
warehouse stage, every open problem as a badge, and the actions for the
user's role. Items and history go in the main column. Merchant, ship-to and
dates go in the side column. The order carries no payment, so the page has
no money summary.

## Review

Open the built screen next to its outline and run the three review checks.
Report each failure as: the question, where the outline puts it, where the
screen puts it.

## Guardrails
- Never invent an actor or a question.
- Never name HTML, CSS, widgets, macros or files in an outline.
- An outline changes before the screen does.
