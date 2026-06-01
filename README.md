# Playlist Chaos

Your AI assistant tried to build a smart playlist generator. The app runs, but some of the behavior is unpredictable. Your task is to explore the app, investigate the code, and use an AI assistant to debug and improve it.

This activity is your first chance to practice AI-assisted debugging on a codebase that is slightly messy, slightly mysterious, and intentionally imperfect.

You do not need to understand everything at once. Approach the app as a curious investigator, work with an AI assistant to explain what you find, and make targeted improvements.

---

## How the code is organized

### `app.py`  

The Streamlit user interface. It handles things like:

- Showing and updating the mood profile  
- Adding songs  
- Displaying playlists  
- Lucky pick  
- Stats and history

### `playlist_logic.py`  

The logic behind the app, including:

- Normalizing and classifying songs  
- Building playlists  
- Merging playlist data  
- Searching  
- Computing statistics  
- Lucky pick mechanics

You will need to look at both files to understand how the app behaves.

---

## What you will do

### 1. Explore the app  

Run the app and try things out:

- Add several songs with different titles, artists, genres, and energy levels  
- Change the mood profile  
- Use the search box  
- Try the lucky pick  
- Inspect the playlist tabs and stats  
- Look at the history  

As you explore, write down at least five things that feel confusing, inconsistent, or strange. These might be bugs, quirks, or unexpected design decisions.

### 2. Ask AI for help understanding the code  

Pick one issue from your list. Use an AI coding assistant to:

- Explain the relevant code sections  
- Walk through what the code is supposed to do  
- Suggest reasons the behavior might not match expectations  

For example:

> "Here is the function that classifies songs. The app is mislabeling some songs. Help me understand what the function is doing and where the logic might need adjustment."

Before making changes, summarize in your own words what you think is happening.

### 3. Fix at least four issues  

Make improvements based on your investigation.

For each fix:

- Identify the source of the issue  
- Decide whether to accept or adjust the AI assistant's suggestions  
- Update the code  
- Add a short comment describing the fix  

Your fixes may involve logic, calculations, search behavior, playlist grouping, lucky pick behavior, or anything else you discover.

### 4. Test your changes  

After each fix, try interacting with the app again:

- Add new songs  
- Change the profile  
- Try search and stats  
- Check whether playlists behave more consistently  

Confirm that the behavior matches your expectations.

### 5. Optional stretch goals  

If you finish early or want an extra challenge, try one of these:

- Improve search behavior  
- Add a "Recently added" view  
- Add sorting controls  
- Improve how Mixed songs are handled  
- Add new features to the history view  
- Introduce better error handling for empty playlists  
- Add a new playlist category of your own design  

---

## Tips for success

- You do not need to solve everything. Focus on exploring and learning.  
- When confused, ask an AI assistant to explain the code or summarize behavior.  
- Test the app often. Small experiments reveal useful clues.  
- Treat surprising behavior as something worth investigating.  
- Stay curious. The unpredictability is intentional and part of the experience.

When you finish, Playlist Chaos will feel more predictable, and you will have taken your first steps into AI-assisted debugging.

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
