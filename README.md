# Writing Studio 🌿 — Android source prototype

**Status: Source implemented, SQLite tests verified, Flutter/Android build NOT run. No APK has been produced.**

A small Android-first, local-fiction-writing app intended for *Testing the Limits*, Chapter 00 / Prologue.

## What is included

- Flutter UI with a Library, manuscript editor, basic Canon Library, and Tools.
- Create universes, books, chapters, characters, and Canon Cards.
- Markdown-first chapter editor, toolbar to insert formatting around selections, rendered Read Mode, Focus Mode, word count and text-size controls.
- Revision-guarded SQLite autosave (650 ms debounce), immutable checkpoints, safe restore, basic side-by-side revision inspection.
- Passage selection → link existing/new Canon Card; reference roles and conservative anchor validation.
- Export chapter Markdown/plain text via Android share sheet.
- Database backup via `VACUUM INTO` and restore from a picked .db with validation and safety copy (restore untested on Android).
- Python standard-library SQLite contract test suite; Android debug build workflow.

## Build on a Flutter-enabled Linux/macOS/Windows runner

The following instructions are intended for a build service, not for the phone-only user:

```bash
flutter create --platforms=android --project-name writing_studio .
flutter pub get
flutter analyze
flutter test
flutter build apk --debug
```

The intended debug APK path, **only if the above build succeeds**, is `build/app/outputs/flutter-apk/app-debug.apk`.

The `.github/workflows/android-debug.yml` workflow expresses this procedure as a manually triggerable GitHub Actions job. This job was not executed in the current environment.

The Honor X8a is an Android 12-era device (actual upgraded OS version can vary). After Android scaffold generation, verify `minSdk` and ABI compatibility rather than guessing the precise firmware.

## Test actual database schema

```bash
python3 tools/generate_statements.py
python3 tools/test_schema.py
```

The SQL parser compiles SQLite triggers into complete statements and stores them in `assets/db/schema_statements.json`. Run the generator after editing SQL.

## Important limitations

- **No installable APK exists in this package.** Network access and the Flutter/Android toolchain were unavailable during authoring. Compilation cannot be asserted.
- The writing screen currently edits visible Markdown source; rich in-place document editing is still an engineering gate. Read Mode renders Markdown.
- Canon Library and relationship timelines are much smaller than the full planned system.
- Named checkpoint comparison shows two versions, but not a line/word diff.
- Only one Main Draft writing branch is visible.
- Search currently uses SQLite `LIKE` instead of FTS5.
- Backups/restore are coded but not verified on Android. Never replace the only copy of valuable writing during first-device testing.
- A fresh database schema is included. Migrations from historic versions are not provided.
- Riverpod is initialized for future separation; UI currently reads a repository provider directly.

See `docs/ARCHITECTURE.md` for the full architecture baseline and outstanding requirements.
