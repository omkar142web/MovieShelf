## AI Poster Metadata JSON Instructions

When analyzing a movie poster image, generate **ONLY** the following JSON structure:

```json
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
```

### Field Rules

#### 1. `title`

* Identify the movie title from the poster.
* Prefer text actually visible on the poster.
* Do not invent or guess a title when it cannot reasonably be identified.
* Keep the original movie title capitalization where possible.

#### 2. `description`

Write a short, useful description of the poster's visual appearance.

Include things such as:

* main characters or subjects
* important objects
* setting/background
* major visual elements
* overall visual atmosphere

Keep it to **1–2 sentences**.

Do not write a plot summary.
Do not add unnecessary movie trivia.

#### 3. `tags`

Generate **5–12 broad searchable tags**.

Tags should describe useful concepts such as:

* genre/style
* major visual themes
* characters or creatures clearly represented
* setting
* atmosphere
* important visual concepts

Examples:
`"superhero"`, `"dark"`, `"action"`, `"sci-fi"`, `"alien"`, `"cinematic"`

Avoid unnecessary duplicates.

#### 4. `keywords`

Generate **5–15 specific search terms** that help a user find the poster.

Keywords can include:

* movie title
* visible characters
* important objects
* locations
* creatures
* visual concepts
* distinctive elements of the poster

Do not simply copy the `tags` list.

Do not add actor names, directors, studios, production companies, or other movie facts unless they are clearly supported by information visible in the poster.

#### 5. `genres`

Provide the most relevant movie genres.

Use **2–5 genres**.

Examples:

* `"Action"`
* `"Drama"`
* `"Science Fiction"`
* `"Thriller"`
* `"Fantasy"`
* `"Crime"`
* `"Comedy"`
* `"Horror"`
* `"Adventure"`
* `"Biography"`
* `"Mystery"`

Do not create overly specific or invented genres.

#### 6. `posterImage.fileName`

This must contain the **exact original filename of the uploaded poster image**.

Important:

* Never rename the file.
* Never change its extension.
* Never invent a filename.
* Preserve spaces, capitalization, numbers, and special characters exactly as provided.

Example:

```json
"posterImage": {
  "fileName": "image5.png"
}
```

#### 7. `category`

Use a simple media category.

For movie posters, use:

```json
"category": "Movie"
```

Do not use `"movie_poster"` unless the application specifically requires that value.

### Important Restrictions

The output must contain **ONLY** these 7 top-level fields:

* `title`
* `description`
* `tags`
* `keywords`
* `genres`
* `posterImage`
* `category`

Do NOT add:

* actors
* director
* release date
* runtime
* studio
* rating
* country
* language
* OCR data
* confidence scores
* colors
* lighting
* composition
* people detection
* object detection
* external IDs
* technical image information
* AI analysis information
* unnecessary metadata

### Image-Grounding Rule

The purpose of this JSON is to create **useful metadata for a local media library**, not to produce a detailed computer-vision report.

Prefer information that is:

1. visible in the poster,
2. clearly identifiable from the poster, or
3. directly useful for searching and organizing the media library.

Do not fill the JSON with information simply because the AI knows it about the movie.

### Output Rule

Return **valid JSON only**.

Do not include:

* Markdown
* explanations
* comments
* ```json code fences
  ```
* additional text before or after the JSON

The final response must be directly parseable by the application.
