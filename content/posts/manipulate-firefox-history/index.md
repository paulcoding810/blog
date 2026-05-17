---
date: "2026-05-17T15:03:59+07:00"
title: "How to Manipulate Firefox History via SQLite"
summary: "A quick guide to directly inserting and querying Firefox browsing history using the SQLite database on macOS."
categories:
  - Code
tags:
  - firefox
  - sqlite
  - productivity
---

## Locate the profile folder

In Firefox, go to `about:support` and look for **Profile Folder**.

On macOS, the path looks like:

```
~/Library/Application Support/Firefox/Profiles/spc65zxa.default-release
```

## Open the history database

The history is stored in the `places.sqlite` SQLite database inside your profile folder. The relevant table is `moz_places`.

Schema of a row in `moz_places`:

```json
{
  "id": 69513,
  "url": "https://www.firefox.com/en-US/",
  "title": "Get Firefox",
  "rev_host": null,
  "visit_count": 2,
  ...
}
```

## Insert a new history entry

Open the database with the SQLite CLI:

```bash
sqlite3 places.sqlite
```

Then insert a row into `moz_places`:

```sql
INSERT INTO moz_places (url, title, visit_count, last_visit_date)
VALUES ('https://github.com', 'GitHub', 1, strftime('%s', 'now') * 1000000);
```

> **Note:** `last_visit_date` is stored in microseconds since the Unix epoch. The `strftime` helper above converts the current time correctly.

You will also need to insert a corresponding row into `moz_historyvisits` to make the entry appear in the browser history view:

```sql
INSERT INTO moz_historyvisits (place_id, visit_date, visit_type)
VALUES (last_insert_rowid(), strftime('%s', 'now') * 1000000, 1);
```

After making changes, close Firefox (or at least ensure it is not writing to the database) so the writes are not overwritten on shutdown.

## Bonus

Simple script to add history quickly:

```bash
#!/usr/bin/env bash
set -euo pipefail

usage() {
  echo "Usage: $0 <url> <title>"
  exit 1
}

if [[ $# -ne 2 ]]; then
  usage
fi

URL="$1"
TITLE="$2"

PROFILE_DIR="$HOME/Library/Application Support/Firefox/Profiles"
PROFILE=$(ls "$PROFILE_DIR" | grep 'default-release' | head -1)

if [[ -z "$PROFILE" ]]; then
  echo "Error: no Firefox default-release profile found." >&2
  exit 1
fi

DB="$PROFILE_DIR/$PROFILE/places.sqlite"

sqlite3 "$DB" <<SQL
INSERT INTO moz_places (url, title, visit_count, last_visit_date)
VALUES ('$URL', '$TITLE', 1, strftime('%s', 'now') * 1000000);

INSERT INTO moz_historyvisits (place_id, visit_date, visit_type)
VALUES (last_insert_rowid(), strftime('%s', 'now') * 1000000, 1);
SQL

echo "Added: $TITLE ($URL)"
```
