# Game Store Rental System

A Python/Jupyter (Google Colab) coursework project for a fictional game store, supporting searching, renting, returning, and booking face-to-face sessions for both video games and board games. Built in first year as part of a programming module, with a file-based (CSV/text) backend rather than a database.

## Features

- **Search** — search video or board game inventory by genre, name, or both, with live availability status
- **Rent** — validates customer ID format and subscription status, game ID format and existence, availability, and the customer's subscription-based rental limit before renting
- **Return** — validates the game is actually on loan, collects a star rating and comments as feedback, and records the return date
- **Face-to-face booking** — books a customer into a time slot, enforcing a maximum store occupancy and rejecting past dates
- **Inventory management** — bar/pie charts of rental frequency per game (via `matplotlib`), and a "suggest deletions" feature flagging rarely-rented games, plus the ability to remove specific copies of a game from inventory
- **GUI** — built with `ipywidgets` (buttons, text inputs, comboboxes, a date picker) so the whole system is operable without touching code

## Structure

- **Database module** — low-level CSV read/write functions (lookups, adding rentals/bookings, deleting inventory rows)
- **gameSearch module** — combines name and genre filters into a single search function
- **gameRent module** — validation logic (customer ID format, subscription status, game ID format/existence, rental limits) plus the rent workflow itself
- **Game return module** — deliberately empty; returning a game reuses the rental module's validation and lookup functions rather than duplicating them (explained in the notebook's own notes)
- **GUI module** — widget definitions, button click handlers, and layout containers tying all of the above together

## Provided modules

`feedbackManager.pyc` and `subscriptionManager.pyc` were supplied by the course as pre-compiled, black-box modules — their internal implementation was intentionally not visible to students, only their documented behavior (checked via Python's `help()` on their functions). They are included here only because the notebook depends on them to run; they are not my own code.

## Data files

Plain CSV-formatted `.txt` files stand in for a database:
- `Video_Game_Info.txt` / `Board_Game_Info.txt` — inventory
- `Rental.txt` — rental history (empty `Return Date` means still on loan)
- `booking.txt` — face-to-face booking records
- `Customer_Subscription_Info.txt` — customer IDs, subscription type, and validity dates
- `Game_Feedback.txt` — star ratings and comments left on return

All customer/game IDs are dummy test data, not real records.

## Running it

This was built and run in Google Colab, with data files expected inside a `content/` subfolder (Colab's default working directory). To run it elsewhere:
1. Create a folder named `content` alongside the notebook
2. Move `Video_Game_Info.txt`, `Board_Game_Info.txt`, `Rental.txt`, `booking.txt`, and `Customer_Subscription_Info.txt` into it
3. Leave `Game_Feedback.txt` and the two `.pyc` files outside `content/`, alongside the notebook itself
4. Run all cells in order

## Testing approach

Rather than a separate test suite, each function is followed in-notebook by `assert` statements exercising its expected behavior and error cases (invalid game types, malformed customer IDs, missing subscriptions, etc.) — run automatically as the notebook executes top to bottom.

## Known limitations

Documented directly in the notebook as a self-assessment of security issues: no sanitization against malicious/oversized input, no authentication for the store-manager interface, no separation of access between modules, and no file-size checks before reading uploaded data. These were identified as part of the coursework's own security-analysis requirement, not fixed in this version.
