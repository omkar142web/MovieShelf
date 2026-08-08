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
  "director": "",
  "posterImage": {
    "fileName": "",
    "path": ""
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
* locating the actual poster file on disk

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

Write a concise description that explains **what the movie is about**.

The description should give the user a quick understanding of the movie's story, subject, or central premise.

Rules:

* Write 1–3 sentences.
* Describe the movie's main premise or story.
* Mention the main subject, conflict, journey, or central idea when useful.
* Keep it concise and informative.
* Do not write a detailed plot summary.
* Do not reveal major spoilers or the ending.
* Do not describe only the poster's visual appearance.
* Do not mention visual elements such as colors, lighting, poster layout, clothing, or background unless they are relevant to explaining the movie itself.
* Use reliable knowledge about the identified movie when necessary.
* If the movie cannot be reliably identified, provide a concise description based only on what can reasonably be determined.

Good:

"A biographical drama about J. Robert Oppenheimer and his role in the development of the atomic bomb during World War II, exploring the scientific ambition, moral consequences, and political tensions surrounding the Manhattan Project."

Bad:

"A man in a suit and hat stands before a massive fiery explosion with Christopher Nolan's name above the title."

The second describes the poster image rather than the movie and should NOT be used as the movie description.

Do not write a full plot summary.

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

## 7. `director`

Store the primary director of the identified movie.

Rules:

* Use the director's real name.
* Use proper capitalization.
* Return only the primary director.
* Do not return an array.
* Do not create an object.
* Do not include assistant directors, producers, writers, or other crew members.
* Director is movie metadata and may be determined from reliable knowledge of the identified movie.
* The director does not need to be visibly printed on the poster.
* If the movie cannot be reliably identified or the director cannot be determined reliably, use an empty string.

Example:

```json
"director": "Christopher Nolan"
```

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

---

## 8. `posterImage`

The `posterImage` object contains the exact original filename and the absolute filesystem path of the poster.

Required structure:

```json
"posterImage": {
  "fileName": "",
  "path": ""
}
```

### `fileName`

Store the exact original uploaded filename.

Rules:

* Never rename the file.
* Never invent the filename.
* Never change the extension.
* Preserve capitalization exactly.
* Preserve spaces, numbers, and special characters exactly.
* Store ONLY the filename.
* Do NOT include folder names.
* Do NOT include `Assets/`.
* Do NOT include the absolute path.

Example:

```json
"fileName": "images.png"
```

### `path`

Store the absolute filesystem path to the actual poster image.

Rules:

* The path must point to the actual existing poster file.
* Use the real absolute path supplied by the application/backend.
* Never guess the path.
* Never construct the path from the movie title.
* Never invent a path.
* Preserve the actual filename and extension.
* Do not use a relative path such as:
  `Assets/images.png`
* Do not use a URL unless the application explicitly uses URLs.
* For Windows paths, use valid JSON escaping for backslashes.

Example:

```json
"path": "C:\\Users\\redmi\\Downloads\\Coding\\AI Media Hub\\Assets\\images.png"
```

IMPORTANT:

The AI model should NOT guess or hallucinate an absolute filesystem path.

The application/backend should provide the actual absolute path.

If the application does not provide the path, return:

```json
"posterImage": {
  "fileName": "",
  "path": ""
}
```

Do not invent one.

---

## 9. `category`

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

**Exception for `cast` and `director`:**  
Tags and keywords should remain useful visual/search metadata. Cast and director are separate movie metadata fields and may use reliable knowledge about the identified movie. Do not put cast or director names into tags merely because they are cast or crew members. Store actor names only in the `cast` field and the director name only in the `director` field.

The absolute poster path is NOT visual metadata.

It must never be inferred from the image.

The application/backend must provide:

* fileName
* path

The vision model should not attempt to determine the filesystem path from the poster.

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
director
posterImage
category
```

Never add additional fields.

The final `posterImage` structure is:

```text
posterImage
├── fileName
└── path
```

Do not add:

* imageWidth
* imageHeight
* fileSize
* mimeType
* checksum
* URL
* thumbnail
* imageId
* OCR
* confidence
* technical image metadata
