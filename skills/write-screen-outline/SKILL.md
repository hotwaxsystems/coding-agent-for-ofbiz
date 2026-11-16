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
   | Details page | Review and act on one record |
   | Create page | Record a new record, one or more steps |
   | Scan page | One field, scan, confirm, next. For handhelds |
   | Dashboard | Counts that say how the work is going, each linked to its list |

5. Place each question. On a details page:
   - Header: record id, state, problems, actions. The first question of
     every actor goes here.
   - Main column, about two thirds wide: the work, such as lines and history.
   - Side column, about one third wide: facts about the record, such as who,
     where and dates.
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
