# AI Poster Metadata JSON Instructions

When analyzing a movie poster image, generate **ONLY** the following JSON structure:

```json
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
```

## Core Principle

Create **compact, useful metadata for a local media library**.

Do not try to describe every detail detected by the vision model.

Every value should be useful for one or more of these purposes:

* displaying the movie
* searching the media library
* filtering the media library
* identifying the poster
* categorizing the media

Prefer **quality over quantity**.

---

## 1. `title`

Identify the movie title.

Rules:

* Prefer the title visibly written on the poster.
* Preserve the correct title and capitalization.
* Do not invent a title.
* Do not use an actor's name as the title.
* If the title cannot reasonably be identified, use an empty string.

Example:

```json
"title": "The Batman"
```

---

## 2. `description`

Write a concise description of the **actual poster image**.

Include the most important:

* main subject/character
* important visual elements
* setting/background
* distinctive objects
* overall visual atmosphere

Rules:

* Use 1–2 sentences.
* Describe what is visually present.
* Do not write a movie plot summary.
* Do not explain the movie's story.
* Do not add unnecessary trivia.
* Do not mention actors unless their identity is clearly printed on the poster and relevant.
* Do not add information only because the AI already knows the movie.

Good:

```text
"A dark superhero poster featuring Batman standing in a rain-soaked city with the Bat-Signal above him and several characters surrounding him."
```

Bad:

```text
"A crime thriller about Bruce Wayne's journey to uncover corruption in Gotham."
```

The second is a plot summary and should NOT be generated.

---

## 3. `tags`

Generate **5–10 useful broad tags**.

Tags should represent high-level searchable concepts such as:

* visual themes
* major subjects
* character types
* creatures
* settings
* atmosphere
* distinctive concepts
* broad genre/style when useful

Examples:

```text
"superhero"
"dark"
"alien"
"symbiote"
"detective"
"gotham"
"bioluminescent"
"historical"
```

Rules:

* Prefer useful tags over generic filler.
* Avoid unnecessary words such as `"poster"` or `"movie"` unless useful.
* Do not repeat the same concept using slightly different words.
* Do not add actor names, directors, studios, or production companies unless they are clearly visible on the poster.
* Do not add unrelated movie trivia.
* Do not add a tag simply to reach the minimum number.

---

## 4. `keywords`

Generate **5–12 specific searchable keywords**.

Keywords should help users find the asset through specific searches.

They may include:

* movie title
* visible characters
* visible creatures
* important objects
* visible locations
* distinctive visual elements
* specific concepts represented in the image

Examples:

```text
"batman"
"bat-signal"
"gotham city"
"masked vigilante"
"rain"
"dark orange sky"
"batmobile"
```

Rules:

* Keywords should be more specific than tags.
* Do not simply copy the tags list.
* Avoid excessive duplication.
* Do not add actor names, directors, studios, production companies, or movie facts unless clearly visible or directly represented in the poster.
* Do not add plot information.
* Do not add information merely because the AI knows the movie.

---

## 5. `genres`

Provide **2–4 relevant movie genres**.

Use common genres only.

Allowed examples include:

```text
"Action"
"Adventure"
"Drama"
"Comedy"
"Crime"
"Fantasy"
"Horror"
"Mystery"
"Romance"
"Science Fiction"
"Thriller"
"Biography"
"History"
```

Rules:

* Use standard genre names.
* Do not create custom genres.
* Do not add a genre just to increase the number.
* If the genre cannot be reliably determined from the poster or known media identification, use only the genres that are reasonably supported.
* Genre should describe the movie, not merely the visual appearance.

---

## 6. `cast`

Generate a list of the principal cast members of the identified movie.

Rules:

* Include approximately 3–8 principal actors.
* Use actor names, not character names.
* Use proper capitalization.
* Do not include directors, writers, producers, studios, or crew.
* Do not include minor/background actors unless they are important principal cast members.
* Keep the value as a simple array of strings.
* Cast is movie metadata and may be determined from reliable knowledge of the identified movie.
* Cast does not need to be visibly printed on the poster.
* If the movie cannot be reliably identified, return an empty array.

Example:

```json
"cast": [
  "Cillian Murphy",
  "Emily Blunt",
  "Matt Damon",
  "Robert Downey Jr.",
  "Florence Pugh"
]
```

---

## 7. `posterImage.fileName`

Store the **exact original uploaded filename**.

Rules:

* Never rename the file.
* Never invent the filename.
* Never change the extension.
* Preserve capitalization exactly.
* Preserve spaces, numbers, and special characters exactly.
* This value must refer to the actual uploaded file.

Example:

```json
"posterImage": {
  "fileName": "images5.png"
}
```

IMPORTANT:

The filename should preferably be provided by the application/backend. Never guess it from the movie title.

---

## 8. `category`

For a movie poster, always use:

```json
"category": "Movie"
```

Use other categories only if the application explicitly supports them.

---

# Important Image-Grounding Rules

The AI must distinguish between:

### A. Information visible in the image

This can be used freely.

Examples:

* Batman
* fire
* blue skin
* explosion
* Bat-Signal
* Golden Gate Bridge
* doctor coat
* sunglasses
* symbiote creature

### B. Information known about the movie but NOT visible in the image

Do NOT automatically add this to visual metadata.

Examples:

* actor names
* director
* studio
* production company
* release year
* character's real name
* plot details
* production history
* awards
* box office
* behind-the-scenes information

Only include such information if it is clearly visible as text on the poster and is genuinely useful for the requested field.

**Exception for `cast`:**  
Tags and keywords should remain useful visual/search metadata. Cast is separate movie metadata and may use reliable knowledge about the identified movie. Do not put cast names into tags merely because they are cast members. Store actor names only in the `cast` field.

---

# Tags vs Keywords

Keep them different.

### Tags = broad concepts

```text
[
  "superhero",
  "dark",
  "crime",
  "detective",
  "gotham",
  "vigilante"
]
```

### Keywords = specific searchable details

```text
[
  "batman",
  "bat-signal",
  "gotham city",
  "masked hero",
  "rain",
  "batmobile"
]
```

Do not produce two lists that are essentially identical.

Do NOT start filling tags and keywords with actor names just because cast information is now available.

For example, this is BAD:

```json
"tags": [
  "superhero",
  "dark",
  "action",
  "tom hardy",
  "michelle williams"
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
  "Michelle Williams",
  "Riz Ahmed"
]
```

---

# Avoid AI Filler

Do NOT add words just to reach a required number.

Bad:

```json
"tags": [
  "cinematic",
  "movie",
  "poster",
  "dramatic",
  "visual",
  "dark",
  "interesting"
]
```

These provide little value.

Prefer:

```json
"tags": [
  "superhero",
  "dark",
  "detective",
  "crime",
  "gotham",
  "vigilante"
]
```

---

# Output Requirements

The output must contain **ONLY valid JSON**.

Do not output:

* Markdown
* code fences
* explanations
* comments
* analysis
* confidence scores
* additional fields
* text before the JSON
* text after the JSON

The JSON must be directly parseable by the application.

The only allowed top-level fields are:

```text
title
description
tags
keywords
genres
cast
posterImage
category
```

Never add additional fields.
