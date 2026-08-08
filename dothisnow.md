Update the existing AI Media Hub frontend.

IMPORTANT:
Do not redesign the UI.
Do not change the JSON structure.
Do not change the existing search/filter behavior except where required below.

CURRENT BEHAVIOR:
The movie details modal already displays cast using:

<div class="detail-section">
  <div class="detail-label">Cast</div>
  <div class="detail-chips">
    <span class="detail-chip">Cillian Murphy</span>
    ...
  </div>
</div>

The main search already searches the cast array using:

...(x.cast || [])

TASK:
Make every cast chip clickable, exactly like the existing tag filter buttons.

==================================================
1. CAST CHIP UI
==================================================

Change cast chips from:

<span class="detail-chip">Cillian Murphy</span>

to a clickable element.

Prefer:

<button
  class="detail-chip cast-chip"
  data-cast="Cillian Murphy"
>
  Cillian Murphy
</button>

Do NOT use an <a> element.

Keep the existing visual appearance of detail-chip.

The cast button should look like the existing chip, not like a normal browser button.

Add:

cursor: pointer;

and appropriate hover styling.

For example:

.detail-chip {
  ...
  cursor: pointer;
}

.cast-chip:hover {
  border-color: #7c5cff;
  color: #fff;
  background: #211b3b;
}

Do not significantly change the existing chip design.

==================================================
2. CLICKING A CAST CHIP
==================================================

When the user clicks a cast chip:

Example:

Cillian Murphy

the application must:

1. Put "Cillian Murphy" into the main search input.
2. Set:

state.search = "Cillian Murphy"

3. Clear the active tag filter.
4. Clear the category filter.
5. Clear the genre filter.
6. Re-render the media results.
7. Keep the search text visible in the search bar.
8. Close the movie details modal.

The final result should behave exactly like the user manually typed:

Cillian Murphy

into the search bar.

==================================================
3. SEARCH BAR SYNCHRONIZATION
==================================================

After clicking:

Cillian Murphy

the search input must visibly contain:

Cillian Murphy

Example:

<input
  id="searchInput"
  ...
  value="Cillian Murphy"
/>

Do not only update the internal state.

The actual visible search bar must update.

==================================================
4. SEARCH BEHAVIOR
==================================================

The existing searchable() function already contains:

...(x.cast || [])

Keep this behavior.

The final searchable fields must remain:

- title
- description
- category
- tags
- keywords
- genres
- cast

Do not remove any of them.

Search must remain:

- case-insensitive
- partial-match capable

Examples:

"Cillian" → Oppenheimer

"Murphy" → Oppenheimer

"Tom Hardy" → Venom

"Robert Pattinson" → The Batman

"Emily" → Oppenheimer

==================================================
5. EVENT HANDLING
==================================================

Because the modal is generated dynamically through openModal(), do NOT attach
click handlers to cast buttons only once during page initialization.

Use event delegation on the modal or modalBox.

For example, add a listener similar to:

$("modalBox").addEventListener("click", (e) => {
  const castButton = e.target.closest(".cast-chip");

  if (!castButton) return;

  const castName = castButton.dataset.cast;

  state.search = castName;
  state.category = "";
  state.genre = "";
  state.tag = "";

  $("searchInput").value = castName;
  $("categoryFilter").value = "";
  $("genreFilter").value = "";

  document
    .querySelectorAll(".tag-btn")
    .forEach((x) => x.classList.remove("active"));

  $("modal").classList.remove("open");

  render();
});

Adapt this to the existing code rather than blindly duplicating code.

==================================================
6. CAST CHIP GENERATION
==================================================

Update the cast portion inside openModal().

Current concept:

(x.cast || [])
  .map((v) => `<span class="detail-chip">${esc(v)}</span>`)
  .join("")

Change it to generate buttons:

(x.cast || [])
  .map(
    (v) =>
      `<button class="detail-chip cast-chip" data-cast="${esc(v)}">${esc(v)}</button>`
  )
  .join("")

Make sure the value is safely escaped because it is inserted into HTML.

==================================================
7. MODAL BEHAVIOR
==================================================

When a cast chip is clicked:

- close modal
- update search bar
- filter results
- show matching movies

Do not reload the page.

Do not navigate to another page.

Do not create a new search page.

==================================================
8. IMPORTANT UX EXPECTATION
==================================================

The user experience should be:

User opens:

Oppenheimer

↓

Sees:

CAST

[Cillian Murphy] [Emily Blunt] [Matt Damon] ...

↓

Clicks:

[Cillian Murphy]

↓

Modal closes

↓

Search bar becomes:

[Cillian Murphy]

↓

Media library automatically filters

↓

Oppenheimer remains visible because it contains Cillian Murphy
in its cast array.

This should feel exactly like clicking a searchable filter.

==================================================
9. DO NOT CHANGE
==================================================

Do not change:

- JSON structure
- mediaData loading
- poster loading
- category filter
- genre filter
- tag filter
- sorting
- clear filters
- card layout
- modal layout
- existing search functionality

Only add the clickable-cast behavior and the necessary styling.

==================================================
10. FINAL TEST
==================================================

After implementing, verify:

1. Open Oppenheimer.
2. Click "Cillian Murphy".
3. Modal closes.
4. Search bar says "Cillian Murphy".
5. Results filter automatically.
6. Oppenheimer appears.
7. Search "Murphy" also finds Oppenheimer.
8. Open another movie and click an actor.
9. The same behavior works.
10. Existing tag buttons still work.
11. Clear all filters still clears the cast-based search.
12. No JavaScript console errors.