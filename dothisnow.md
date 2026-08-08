I need you to update my AI Media Hub project in TWO places.

Do not redesign the application or change unrelated functionality.

TASK 1:
Update the movie metadata JSON data format and all existing JSON records.

TASK 2:
Update the AI Poster Metadata JSON Instructions markdown file so future AI-generated metadata follows the new format.

==================================================
TASK 1 — UPDATE THE JSON FORMAT
==================================================

The current JSON structure is:

{
  "title": "",
  "description": "",
  "tags": [],
  "keywords": [],
  "genres": [],
  "posterImage": {
    "fileName": ""
  },
  "category": ""
}

Change it to:

{
  "title": "",

  "description": "",

  "tags": [],

  "keywords": [],

  "genres": [],

  "cast": [],

  "posterImage": {
    "fileName": ""
  },

  "category": ""
}

The ONLY new metadata field is:

"cast": []

Do not add or remove any other metadata fields.

==================================================
CAST FIELD RULES
==================================================

"cast" must be a simple array of principal actor names.

Example:

"cast": [
  "Cillian Murphy",
  "Emily Blunt",
  "Matt Damon",
  "Robert Downey Jr.",
  "Florence Pugh"
]

Rules:

- Use real actor names.
- Include the main/principal cast only.
- Prefer approximately 3–8 important cast members.
- Do not include dozens of minor actors.
- Do not include directors, writers, producers, studios, or crew.
- Do not include character names.
- Do not create objects such as:
  {
    "actor": "",
    "character": ""
  }
- Keep it as a simple string array.
- Preserve proper capitalization of names.
- If the movie cannot be reliably identified, use an empty array.
- Cast is movie metadata and is allowed to come from reliable knowledge about the identified movie. It does NOT have to be visibly printed on the poster.

==================================================
UPDATE ALL EXISTING JSON DATA
==================================================

Find the current movie metadata JSON/data used by the application.

Add the "cast" field to every existing movie record.

For example:

Avatar:

"cast": [
  "Sam Worthington",
  "Zoe Saldana",
  "Sigourney Weaver",
  "Stephen Lang",
  "Michelle Rodriguez"
]

Oppenheimer:

"cast": [
  "Cillian Murphy",
  "Emily Blunt",
  "Matt Damon",
  "Robert Downey Jr.",
  "Florence Pugh"
]

Venom:

"cast": [
  "Tom Hardy",
  "Michelle Williams",
  "Riz Ahmed",
  "Scott Haze",
  "Reid Scott"
]

Kabir Singh:

"cast": [
  "Shahid Kapoor",
  "Kiara Advani",
  "Arjan Bajwa",
  "Suresh Oberoi"
]

The Batman:

"cast": [
  "Robert Pattinson",
  "Zoë Kravitz",
  "Paul Dano",
  "Jeffrey Wright",
  "Colin Farrell",
  "Andy Serkis"
]

Keep all existing fields and values unless a change is specifically required by this task.

Do not remove:
- title
- description
- tags
- keywords
- genres
- posterImage
- category

==================================================
UPDATE THE WEB INTERFACE
==================================================

The cast must be visible in the movie details modal.

Current modal sections include things such as:

- Description
- Genres
- Tags
- Keywords
- Poster file

Add:

- Cast

Place Cast after Genres and before Tags.

Example:

CAST

Cillian Murphy
Emily Blunt
Matt Damon
Robert Downey Jr.
Florence Pugh

Use the same visual chip/pill style already used for genres/tags/keywords.

Do not create a separate page for cast.

==================================================
UPDATE SEARCH
==================================================

The existing search bar currently searches fields such as:

- title
- description
- tags
- keywords
- genres
- category

Update the search logic so it ALSO searches:

- cast

For example:

Searching:

"Cillian Murphy"

must return:

Oppenheimer

Searching:

"Tom Hardy"

must return:

Venom

Searching:

"Robert Pattinson"

must return:

The Batman

Searching:

"Emily Blunt"

must return:

Oppenheimer

The search must be case-insensitive.

It should search across:

title
description
tags
keywords
genres
cast
category

Do not remove any existing search functionality.

==================================================
UPDATE FILTER / UI LOGIC
==================================================

Do NOT create a cast filter in the sidebar yet.

Cast only needs to:

1. appear in the details modal
2. be searchable through the main search bar

Keep the existing:

- category filter
- genre filter
- tag filter
- sorting
- clear filters
- poster cards
- details modal

working exactly as they currently do.

==================================================
TASK 2 — UPDATE THE AI INSTRUCTIONS MARKDOWN
==================================================

Find the existing file:

AI Poster Metadata JSON Instructions

Update it so the required JSON format becomes:

{
  "title": "",
  "description": "",
  "tags": [],
  "keywords": [],
  "genres": [],
  "cast": [],
  "posterImage": {
    "fileName": ""
  },
  "category": ""
}

The only additional top-level field is:

"cast": []

==================================================
ADD THIS SECTION TO THE INSTRUCTIONS
==================================================

## 6. `cast`

Generate a list of the principal cast members of the identified movie.

Rules:

- Include approximately 3–8 principal actors.
- Use actor names, not character names.
- Use proper capitalization.
- Do not include directors, writers, producers, studios, or crew.
- Do not include minor/background actors unless they are important principal cast members.
- Keep the value as a simple array of strings.
- Cast is movie metadata and may be determined from reliable knowledge of the identified movie.
- Cast does not need to be visibly printed on the poster.
- If the movie cannot be reliably identified, return an empty array.

Example:

"cast": [
  "Cillian Murphy",
  "Emily Blunt",
  "Matt Damon",
  "Robert Downey Jr.",
  "Florence Pugh"
]

==================================================
RENUMBER THE EXISTING SECTIONS
==================================================

After adding cast:

1. title
2. description
3. tags
4. keywords
5. genres
6. cast
7. posterImage.fileName
8. category

Update all references in the markdown accordingly.

==================================================
UPDATE THE IMAGE-GROUNDING RULE
==================================================

The previous instructions said that actor names should not be added because they are not necessarily visible on the poster.

Keep that rule for tags and keywords.

However, create an explicit exception for the new "cast" field:

- Tags and keywords should remain useful visual/search metadata.
- Cast is separate movie metadata.
- Cast may use reliable knowledge about the identified movie.
- Do not put cast names into tags merely because they are cast members.
- Store actor names only in the "cast" field.

This distinction is important.

==================================================
UPDATE TAGS / KEYWORDS RULE
==================================================

Keep the existing rules for tags and keywords.

Do NOT start filling tags and keywords with actor names just because cast information is now available.

For example, this is BAD:

"tags": [
  "superhero",
  "dark",
  "action",
  "tom hardy",
  "michelle williams"
]

Instead:

"tags": [
  "superhero",
  "symbiote",
  "dark",
  "action"
],

"cast": [
  "Tom Hardy",
  "Michelle Williams",
  "Riz Ahmed"
]

==================================================
FINAL ALLOWED JSON STRUCTURE
==================================================

The AI output must contain ONLY these 8 top-level fields:

title
description
tags
keywords
genres
cast
posterImage
category

No other fields are allowed.

Do NOT add:

- director
- actors object
- character names
- release date
- runtime
- studio
- rating
- country
- language
- OCR
- confidence
- colors
- lighting
- composition
- people detection
- object detection
- external IDs
- technical image information
- AI analysis information

==================================================
IMPORTANT
==================================================

After making the changes:

1. Find the actual JSON/data file used by the application.
2. Add "cast" to every existing record.
3. Find the actual AI Poster Metadata JSON Instructions markdown file.
4. Update the markdown instructions.
5. Update the frontend modal to display cast.
6. Update search so cast is searchable.
7. Verify there are no JavaScript errors.
8. Verify all existing search/filter functionality still works.
9. Verify an actor search returns the correct movie.
10. Do not make unrelated UI or architectural changes.

Do not just explain the changes to me.

Actually modify the project files.