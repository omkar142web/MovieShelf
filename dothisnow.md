Yes — **add `director`**. For your project, it is one of the few metadata fields that is genuinely useful for a movie owner.

Your current schema already has `cast`, and your HTML already searches/displays cast. 

I would add `director` as a **simple string**, not an array or object:

```json
"director": "Christopher Nolan"
```

And importantly, **director should be movie metadata**, just like cast. It does not need to be visible on the poster.

Give your AI IDE this exact instruction:

---

# AI IDE INSTRUCTION — ADD DIRECTOR

````text
Update my AI Media Hub project to add a new `director` field to the movie metadata.

IMPORTANT:

Do not redesign the application.

Do not remove any existing fields.

Do not change the existing tags, keywords, genres, cast, posterImage, or category behavior.

The new field must be:

"director": ""

It must be a simple string containing the primary movie director's name.

==================================================
TASK 1 — UPDATE AI POSTER METADATA JSON INSTRUCTIONS
==================================================

Find the existing file:

AI Poster Metadata JSON Instructions

Update the JSON structure at the top.

CURRENT:

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

CHANGE TO:

{
  "title": "",
  "description": "",
  "tags": [],
  "keywords": [],
  "genres": [],
  "cast": [],
  "director": "",
  "posterImage": {
    "fileName": ""
  },
  "category": ""
}

The only new top-level field is:

"director": ""

==================================================
ADD A NEW DIRECTOR SECTION
==================================================

Add this section after `cast` and before `posterImage.fileName`.

## 7. `director`

Store the primary director of the identified movie.

Rules:

- Use the director's real name.
- Use proper capitalization.
- Return only the primary director.
- Do not return an array.
- Do not create an object.
- Do not include assistant directors, producers, writers, or other crew members.
- Director is movie metadata and may be determined from reliable knowledge of the identified movie.
- The director does not need to be visibly printed on the poster.
- If the movie cannot be reliably identified or the director cannot be determined reliably, use an empty string.

Example:

```json
"director": "Christopher Nolan"
````

Do NOT produce:

```json
"director": [
  "Christopher Nolan"
]
```

Do NOT produce:

```json
"director": {
  "name": "Christopher Nolan"
}
```

Only use:

```json
"director": "Christopher Nolan"
```

==================================================
IMPORTANT IMAGE-GROUNDING RULE UPDATE
=====================================

The existing instructions contain a section explaining the difference between:

A. Information visible in the image

B. Information known about the movie but not visible in the image

Keep that distinction.

However, update the exception so BOTH `cast` and `director` are treated as movie metadata.

Use this rule:

"Cast and director are separate movie metadata fields. They may use reliable knowledge about the identified movie and do not need to be visibly printed on the poster."

Tags and keywords must NOT automatically contain actor or director names.

For example, this is BAD:

```json
"tags": [
  "superhero",
  "dark",
  "action",
  "tom hardy",
  "director"
]
```

Instead:

```json
"tags": [
  "superhero",
  "symbiote",
  "dark",
  "action"
],
"cast": [
  "Tom Hardy",
  "Michelle Williams"
],
"director": "Ruben Fleischer"
```

==================================================
UPDATE THE ALLOWED TOP-LEVEL FIELDS
===================================

The final allowed top-level fields are:

title
description
tags
keywords
genres
cast
director
posterImage
category

Never add:

* actors
* director object
* directors
* writer
* writers
* producer
* studio
* release date
* runtime
* rating
* country
* language
* OCR
* confidence
* colors
* lighting
* composition
* people detection
* object detection
* external IDs
* technical image information
* AI analysis information

==================================================
RENUMBER THE SECTIONS
=====================

The final sections should be:

1. title
2. description
3. tags
4. keywords
5. genres
6. cast
7. director
8. posterImage.fileName
9. category

Update all references in the markdown accordingly.

==================================================
TASK 2 — UPDATE THE EXISTING METADATA JSON
==========================================

Find the actual metadata JSON file currently used by the application.

The current HTML loads:

meta-data-2.json

Update every existing movie object to include:

"director": ""

Place the field after `cast`.

For the current movies, use:

Avatar:

"director": "James Cameron"

Oppenheimer:

"director": "Christopher Nolan"

Venom:

"director": "Ruben Fleischer"

Kabir Singh:

"director": "Sandeep Reddy Vanga"

The Batman:

"director": "Matt Reeves"

Keep all existing metadata unchanged unless required to add the new field.

The resulting structure must look like:

{
"title": "Oppenheimer",

"description": "...",

"tags": [],

"keywords": [],

"genres": [],

"cast": [
"Cillian Murphy",
"Emily Blunt",
"Matt Damon",
"Robert Downey Jr.",
"Florence Pugh"
],

"director": "Christopher Nolan",

"posterImage": {
"fileName": "Assets/images1.png"
},

"category": "Movie"
}

==================================================
TASK 3 — MAKE DIRECTOR ACTUALLY USEFUL IN THE WEB APP
=====================================================

This is REQUIRED because adding the field to JSON alone would make it unused.

The current application already searches:

* title
* description
* category
* tags
* keywords
* genres
* cast

Update the existing `searchable()` function to ALSO include:

x.director

The final searchable fields must be:

* title
* description
* category
* tags
* keywords
* genres
* cast
* director

Therefore:

Searching:

"Christopher Nolan"

must return:

Oppenheimer

Searching:

"Nolan"

must return:

Oppenheimer

Searching:

"Matt Reeves"

must return:

The Batman

Searching:

"James Cameron"

must return:

Avatar

Search must remain case-insensitive and support partial matches.

==================================================
TASK 4 — SHOW DIRECTOR IN THE MOVIE MODAL
=========================================

The current modal displays:

* Description
* Genres
* Cast
* Tags
* Keywords
* Poster file

Add:

DIRECTOR

Example:

DIRECTOR

Christopher Nolan

Place Director immediately after the movie category/meta information and before Description.

Do NOT make director a chip.

It should simply display as a normal movie-information value.

Example:

<div class="detail-section">
  <div class="detail-label">Director</div>
  <div class="modal-description">Christopher Nolan</div>
</div>

You may create a small dedicated class if necessary, but keep it visually consistent with the existing UI.

==================================================
TASK 5 — OPTIONAL CLICK BEHAVIOR
================================

Make the displayed director clickable like the cast chip ONLY if it can be done cleanly using the existing search architecture.

If implemented:

Clicking:

Christopher Nolan

should:

1. Put "Christopher Nolan" into the search bar.
2. Set state.search to "Christopher Nolan".
3. Clear category, genre, and tag filters.
4. Close the modal.
5. Render the filtered results.

Use the existing search system rather than creating a separate director filter.

Do not create a Director sidebar filter yet.

==================================================
FINAL VALIDATION
================

After making the changes, verify:

1. Every metadata object contains `director`.
2. `director` is a string.
3. The AI instructions contain the director field.
4. The AI instructions explain director correctly.
5. Existing fields remain unchanged.
6. Search for "Christopher Nolan" returns Oppenheimer.
7. Search for "Nolan" returns Oppenheimer.
8. Search for "Matt Reeves" returns The Batman.
9. Director appears in the movie modal.
10. Cast search still works.
11. Tag search/filter still works.
12. Genre filtering still works.
13. Clear filters still works.
14. No JavaScript console errors occur.
15. Do not make unrelated changes to the project.

````

### One correction I'd make to your schema philosophy

Your final metadata is now becoming:

```json
{
  "title": "",
  "description": "",
  "tags": [],
  "keywords": [],
  "genres": [],
  "cast": [],
  "director": "",
  "posterImage": {
    "fileName": ""
  },
  "category": ""
}
````

I think this is a **very good stopping point** for the movie metadata layer.

You now have:

* **Title** → identification
* **Description** → what the movie is about
* **Tags** → broad discovery
* **Keywords** → detailed search
* **Genres** → filtering
* **Cast** → actor-based search
* **Director** → filmmaker-based search
* **Poster** → visual asset
* **Category** → organization

I would **not add release date, runtime, studio, rating, country, language, IMDb ID, etc. yet**. Those can be added later if the owner's actual workflow proves they are needed.
