Yes — but one important point: **do not store the absolute path inside `posterImage.fileName` if this JSON is meant to be portable.** If your local app specifically needs the absolute filesystem path to load the image, then use a separate field such as `posterImage.path`. However, you asked to update the existing format to use the absolute path, so below I’ll make that explicit and keep the filename too.

For example:

```json
"posterImage": {
  "fileName": "images.png",
  "path": "C:\\Users\\redmi\\Downloads\\Coding\\AI Media Hub\\Assets\\images.png"
}
```

This is better than replacing the filename because your UI can still display the filename while the application uses `path` to locate the actual file.

Give your AI IDE this instruction:

````text
Update my AI Media Hub project in exactly TWO places:

1. Update the `AI Poster Metadata JSON Instructions` markdown file.
2. Update the actual movie metadata JSON file (`meta-data-2.json`).

Do not make unrelated changes.

==================================================
IMPORTANT: POSTER IMAGE PATH
==================================================

The current metadata uses:

"posterImage": {
  "fileName": "Assets/images.png"
}

This is not sufficient for my local application because the application needs to know the actual filesystem location of the poster.

Change `posterImage` to:

"posterImage": {
  "fileName": "images.png",
  "path": "C:\\Users\\redmi\\Downloads\\Coding\\AI Media Hub\\Assets\\images.png"
}

The exact absolute path must point to the actual uploaded/local poster file.

IMPORTANT:

- `fileName` = exact original filename only.
- `path` = absolute filesystem path to the actual poster.
- Do NOT put `Assets/` inside `fileName`.
- Do NOT invent paths.
- Do NOT guess paths.
- Use the actual project/file location.

==================================================
TASK 1 — UPDATE THE JSON INSTRUCTIONS MARKDOWN
==================================================

Find:

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
  "director": "",
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
    "fileName": "",
    "path": ""
  },
  "category": ""
}

The ONLY new field inside `posterImage` is:

"path": ""

Do not add any other poster/image metadata.

==================================================
UPDATE SECTION 8 — posterImage
==================================================

Replace the existing `posterImage.fileName` section with:

## 8. `posterImage`

The `posterImage` object contains the exact original filename and the absolute filesystem path of the poster.

Required structure:

```json
"posterImage": {
  "fileName": "",
  "path": ""
}
````

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

==================================================
UPDATE THE CORE PRINCIPLE
=========================

Add this to the Core Principle:

Poster metadata must identify both:

1. the original poster filename
2. the actual local filesystem location of the poster

The filename and path serve different purposes.

`fileName` is the asset's original filename.

`path` is the actual location used by the local application.

==================================================
UPDATE THE IMAGE-GROUNDING RULES
================================

The absolute poster path is NOT visual metadata.

It must never be inferred from the image.

The application/backend must provide:

* fileName
* path

The vision model should not attempt to determine the filesystem path from the poster.

==================================================
UPDATE THE ALLOWED STRUCTURE
============================

The final allowed top-level fields remain:

title
description
tags
keywords
genres
cast
director
posterImage
category

The final `posterImage` structure is:

posterImage
├── fileName
└── path

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

==================================================
TASK 2 — UPDATE meta-data-2.json
================================

Find the actual:

meta-data-2.json

used by the application.

Update every movie record.

Current:

"posterImage": {
"fileName": "Assets/images.png"
}

Change to:

"posterImage": {
"fileName": "images.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images.png"
}

Do the same for every movie.

Use the ACTUAL absolute paths of the files in the project.

For example, if the files are physically located at:

C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images.png

C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images1.png

C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images2.png

C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images4.png

C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\image5.png

then the metadata should use:

Avatar:

"posterImage": {
"fileName": "images.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images.png"
}

Oppenheimer:

"posterImage": {
"fileName": "images1.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images1.png"
}

Venom:

"posterImage": {
"fileName": "images2.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images2.png"
}

Kabir Singh:

"posterImage": {
"fileName": "images4.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\images4.png"
}

The Batman:

"posterImage": {
"fileName": "image5.png",
"path": "C:\Users\redmi\Downloads\Coding\AI Media Hub\Assets\image5.png"
}

IMPORTANT:

Do not blindly use these example paths if the actual project is located somewhere else.

First inspect the actual project structure and use the real absolute path.

==================================================
TASK 3 — UPDATE THE HTML APPLICATION
====================================

The current HTML loads poster images using:

x.posterImage?.fileName

This currently expects a relative path.

After changing the JSON, update the application so that poster loading uses:

x.posterImage?.path

when an absolute path is available.

However, because browsers have security restrictions around arbitrary local filesystem paths, do NOT assume that putting a Windows filesystem path directly into:

<img src="C:\\Users\\...">

will work in a normal browser.

Inspect the current application architecture.

If the project is being served by a local web server, expose the Assets directory through that server and use the appropriate local URL.

If the application is running directly as a local HTML file, determine the safest working approach for loading the local assets.

Do NOT break existing poster loading.

The metadata should preserve the absolute path even if the frontend needs a different browser-accessible URL.

==================================================
TASK 4 — DO NOT CHANGE MOVIE METADATA
=====================================

Keep these existing fields:

* title
* description
* tags
* keywords
* genres
* cast
* director
* category

Do not remove or rename them.

Do not add unrelated fields.

==================================================
FINAL STRUCTURE
===============

Every movie must ultimately follow:

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
"category": "Movie"
}

==================================================
FINAL VALIDATION
================

After making the changes:

1. Verify the markdown instructions contain `posterImage.fileName`.
2. Verify the markdown instructions contain `posterImage.path`.
3. Verify the markdown explicitly says the absolute path must come from the application/backend.
4. Verify every movie in `meta-data-2.json` has both fields.
5. Verify `fileName` contains ONLY the filename.
6. Verify `path` contains the absolute filesystem path.
7. Verify all paths point to real existing files.
8. Verify poster images still display in the application.
9. Verify search still works.
10. Verify cast still works.
11. Verify director still works.
12. Do not make unrelated changes.

```

One **important architectural warning**: your current HTML is a browser app and currently uses `fetch('meta-data-2.json')` plus `img src` paths. :contentReference[oaicite:0]{index=0} An absolute Windows path in JSON is **not automatically a browser-loadable image URL**. So I would absolutely have the IDE inspect how you're running the app before changing the `<img src>` logic.

For the **metadata**, absolute `path` is fine if your local backend/desktop app needs it. For the **browser**, the actual serving mechanism should determine how that path gets converted into something the browser can load.
```
