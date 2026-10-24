# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Added

### Fixed
- Fix non-standard lexicon fields and add missing documentation guidance (#1)

### Changed
- `#pronounSet.forms` items: `maxLength` raised from 64 to 240 bytes, so 24 graphemes of CJK, Hangul or emoji fit. `maxGraphemes` stays 24. Records that were valid before stay valid.
