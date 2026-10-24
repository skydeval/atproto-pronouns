# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Added
- Design docs in `docs/design/` (`overview.md`, `record.md`, `display.md`), holding the spec that was in the README.
- Documented clearing versus deleting: an empty `sets` hides pronouns and blocks the fallback to the Bluesky profile field; a deleted record allows it.
- Documented that unknown and not-yet-loaded states show nothing, so stale pronouns never flash.

### Fixed
- Fix non-standard lexicon fields and add missing documentation guidance (#1)

### Changed
- `#pronounSet.forms` items: `maxLength` raised from 64 to 240 bytes, so 24 graphemes of CJK, Hangul or emoji fit. `maxGraphemes` stays 24. Records that were valid before stay valid.
- README shortened to one screen, with a note that it works alongside atproto-plural (optional). Screenshots moved to `docs/images/`.
