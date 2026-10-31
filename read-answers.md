---
name: watcher-reports
description: >-
  Read stored Watcher form answers and turn them into a table or a report.
  Use when someone asks what was submitted, wants a summary, or wants those
  answers emailed, filed, or sent on a schedule.
---

# Watcher answers

The Watcher server stores the answers. You read them. You do not write the database and you do not ask the server to email, upload, or build a PDF.

The person's email, calendar, Drive, and routines are yours. If they say "every hour, import the new answers and email them to me", use your own schedule and your own mailbox. The server has neither.

## How to read

1. Call `submissions_list` for their watch.
2. One answer: call `submission_get` and show the fields as they were saved. Do not invent a value. Do not drop a field.
3. Several answers: build one table. One row per answer. Columns are the id, the form, and each saved field. Keep every row. If a field is missing on a row, leave that cell empty.
4. Show that table and wait. The person says what to do with it: email, Drive, a PDF, or a schedule.

## Do not

- Ask the server to push a webhook or to write a Google Sheet.
- Calculate dates or fill a blank with a guess.
- Mix answers from two people into one row.
