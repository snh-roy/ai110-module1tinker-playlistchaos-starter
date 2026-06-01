# Playlist Chaos

---

## Lab Report

### 1. Bug Fixes

**What was observed in the app?**
I observed that songs with an energy level of 7 were being categorized as Mixed instead of Hype. Additionally, songs with keywords like "Sleep" in the title (e.g., "Sleep Sound") were failing to be classified as Chill, staying in the Mixed category instead. Other issues included case-sensitive search, the Lucky Pick ignoring Mixed songs, incorrect statistics calculations, and lack of input normalization.

**Which file and function was investigated?**
I primarily investigated playlist_logic.py and the classify_song, search_songs, lucky_pick, and compute_playlist_stats functions. I also updated app.py for UI-related stats formatting and input normalization.

**What specifically was wrong in the code?**
- In classify_song, the energy comparison used > instead of >= for the Hype threshold, and title keyword matching was case-sensitive.
- In search_songs, the partial match logic was robust but the UI only allowed searching by artist.
- In lucky_pick, the "any" mode explicitly excluded the "Mixed" playlist category.
- In compute_playlist_stats, songs were being double-counted if they appeared in multiple lists, and the hype ratio was not a percentage.
- In the "Add Song" handler in app.py, inputs were not being stripped of whitespace or lowercased consistently.

**How was the fix tested?**
I tested the fixes by:
- Adding a song with exactly 7 energy and confirming it appeared in Hype.
- Adding a song titled "Sleep Sound" and verifying it was correctly classified as Chill.
- Searching for "ac" and confirming "AC/DC" was found.
- Using "Feeling lucky" with "any" mode and confirming Mixed songs could be picked.
- Verifying the stats dashboard showed accurate counts and percentage ratios.

### 2. AI Assistant Usage

**Asking "what does this do" before "fix this"**
Yes, I conducted a thorough analysis of the functions before implementing changes. I reviewed the implementation of classify_song, lucky_pick, and compute_playlist_stats to understand the intended versus actual logic before requesting or applying fixes.

**AI Errors or Incompleteness**
The AI assistant was generally accurate but initially followed the existing pattern in lucky_pick where Mixed songs were excluded. I had to identify that this behavior contradicted the user's expectation for an "any" selection, leading to a more comprehensive fix.

**Rephrasing for better results**
I had to be very specific when discussing the statistics bug. Initially, the logic might have simply summed the lengths of the lists, but I specified that deduplication based on title and artist was necessary to handle potential double-counting from merged playlists.

### 3. Refactoring

**What was refactored and why?**
I chose to refactor the classify_song function and extract the keyword matching logic into a helper function named matches_any. This improved the readability of the main classification logic and made the intent of the keyword checks more explicit.

**Rejected AI suggestions**
I rejected a suggestion to use a complex dictionary-based mapping for moods because the current procedural logic clearly defines the priority of Hype over Chill. A dictionary mapping would have added unnecessary complexity.

**Verification of the refactor**
I confirmed the refactor did not break anything by re-validating the same edge cases used during the bug fix phase. I verified that energy 7 songs remained Hype and capitalized titles still triggered the Chill classification.
