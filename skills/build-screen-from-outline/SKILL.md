---
name: build-screen-from-outline
description: |
  Build an OFBiz screen from its screen outline, so the page answers each
  actor's questions in the place the outline names. Use when
  - Building a new screen that has a screen outline.
  - Changing a built screen after its outline changed.
  - Checking a built screen against its outline before a pull request.
---

## Goal
The built page matches its screen outline and its mockup.
The first question of every actor is answered in one look, with no scrolling.

## Inputs
Read these before writing code:
1. The screen outline. It names the actors, their questions in order, the
   page type, where each question is answered, and the data block for each
   place.
2. The mockup, when the project has one. It is the picture of the page.
   Match its layout and its components. Never copy its sample data or its
   scripts.
3. The project's rules for screens, such as the decorator, the style kit
   and the data block macros.
4. The manage-templates skill, its "Data block macros" section.

## Method
1. Find the page type in the outline. Build that layout from the table
   under "Page layouts".
2. Place each question where the outline puts it. Build each place with the
   data block the outline names.
3. Write the screen's Groovy data script. It loads everything the page and
   its data blocks need, once, before the page is drawn. A template never
   queries the database.
4. Use the project's decorator. A screen fills only the page area. It never
   draws its own menu, top bar or warehouse choice.
5. Use the project's style kit as it comes. Add a custom class only when the
   kit has no fit, and say why in the pull request.
6. Every word on the page comes from a label.
7. Run the review checks below on the running page.

## Page layouts

| Page type | Layout |
| :--- | :--- |
| Index page | Header: title, result count, the required constraint on the find form with its default filled in. Body: one table, the default sort, pagination. The first column links to the details page |
| Details page | Header: record code, state, problems, actions. Then a wide main column, about two thirds, for the work, and a narrow side column, about one third, for facts about the record |
| Create page | One column, about two thirds wide. Cards in the order people fill them. One Save and Cancel at the bottom. After Save, the new record's details page opens |
| Scan page | One column at phone width, no menu bar. Header in large text: where to go and what to take. The scan field is always focused. Large buttons for one thumb |
| Desk page | One column at a computer. One tab for each part of the job. Keyboard first: Enter moves to the next field |
| Report page | Header: scope, time choice, the counts that answer question 1. One tab per view; the tab for question 1 opens first. Actions in a side column |
| Dashboard | Header: scope and time window. Work that needs a person comes first. Rows of count tiles, full width. Each tile opens the list behind its count |
| Items into containers | A "not placed yet" list under the header, then one card per container. A scan puts the item into the open container |

## Lists
- Every list has a required constraint, a default sort and pagination. An
  empty filter never means all records.
- The constraint shows on the find form with its default. The user may
  change it within its limits.
- Filter and sort only on fields a database index serves. Search one field
  the user chooses, never a field guessed from the value typed.
- Search only on exact ids and codes, never on names, emails or notes.

## Partial updates
- When one part of a page changes, render that part as its own small screen
  that can be drawn alone, and swap it in.
- Keep page scripts small and declared in the markup. Do not write page
  scripts by hand where the project's library covers it.

## Known traps
- Bootstrap's `d-flex` class uses `!important`, so it beats Alpine's
  `x-show`. On an element that must hide, switch the class instead:
  `:class="shown ? 'd-flex' : 'd-none'"`.
- An `<option>` cannot hold HTML, so a data block macro cannot draw inside
  a select. Show plain "code · name" text there.

## Review checks
Run these on the running page, signed in as each actor the outline names:
1. Each question is answered in the place the outline names.
2. Question 1 of each actor is answered in one look, with no scrolling.
3. Anything on the page that answers no question moves down, hides, or goes.
4. Every business value is drawn by its data block.
5. The page shows no raw ids, empty values or template errors.

## Guardrails
- Never add a question, a field or an action the outline does not name.
  Raise it as an open question for the designer.
- Never let a template query the database.
- Never draw a value by hand that a data block covers.
