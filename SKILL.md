---
name: limoncello-forms
description: >-
  Turn a Google sheet or a written list of questions into a voice form and
  publish it to the Limoncello server. Use when someone wants a form on the watch.
---

# Limoncello forms

The server owns the form. Do not write the database or the voice bridge yourself. Use the Limoncello MCP tools.

## Steps

1. Read the questions from a sheet or a document. If a Google Form cannot be read directly, ask for the sheet or the document. That gap is still open.
2. Build SurveyJS JSON:
   - One page per section. A section has at most 15 fields.
   - Short answers are `text`. Long stories are `comment`. A yes or no is `boolean`.
   - Choices are `dropdown` with at most 6 short options.
   - Dates, times, and numbers are `text` with `inputType` `date`, `time`, or `number`.
   - A photo moment is `photo`, after the name field, with `staffField` set to that name. The watch shows a QR there. The phone takes the photos. Do not ask for a camera on the watch.
   - Calculated rules are only `same_place_if_unsaid`, `return_day_from_days`, and `days_from_two_dates`. Do not invent a new rule. Do not calculate a date yourself.
   - Leave out anything voice cannot ask, such as matrices or other files, and list why.
3. Show the JSON and the list of leftovers. Wait until the person approves.
4. Publish, test, and assign with the calls below. Do not publish before they approve.

## Calls

`form_publish` takes two arguments. `id` is a short name. `survey` is the approved JSON as an object. `pages` is a property of that object. Each page has `elements`. Pass the object on `survey`. Do not wrap it in `form`, `json`, or `surveyJson`. Do not send `survey` as a text string. Do not try other argument names.

```json
{
  "id": "visit",
  "survey": {
    "title": "Visit",
    "pages": [
      {
        "name": "who",
        "elements": [
          { "type": "text", "name": "staff_name", "isRequired": true },
          { "type": "photo", "name": "photos", "staffField": "staff_name" }
        ]
      }
    ]
  }
}
```

`published visit` means it is stored. `a form needs at least one page` means `survey.pages` was missing. Send the approved object on `survey` once more. Stop if that answer comes back again.

`form_test` takes `id` and `turns`. `turns` is an array, one turn per section. Each turn is `{ "patch": { "staff_name": "Ana" } }`.

`form_assign` takes `formId`. Add `watchId` only when the account has more than one watch.

`form_link` takes `formId`. Call `form_get` first. If the owner named a client and the form has a field for that name, also pass `name` and `field`. `field` is the exact element name from `form_get`. Do not guess it. If there is no such field, omit `field`. The answer is a URL. `form_link_revoke` takes `name` or `id` and closes that link.
