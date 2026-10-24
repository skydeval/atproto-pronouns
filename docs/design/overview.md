# atproto-pronouns: design overview

`app.nearhorizon.actor.pronouns` stores a user's pronouns as a record in their
own ATProto repository.

| Document | Covers |
| --- | --- |
| [overview.md](overview.md) | Why the record exists and the principles behind it. |
| [record.md](record.md) | The record's fields, limits, writing rules and examples. |
| [display.md](display.md) | How clients show pronouns, including fallback and empty states. |

## Why this exists

Bluesky's profile record has a `pronouns` field: a single free-form string
capped at 20 graphemes. It works for "she/her". It doesn't work for multiple
sets, neopronouns, or anything longer.

Labelers are someone else's server. When a popular pronoun labeler was
deleted, everyone's pronouns went with it.

This record lives on the user's PDS. It holds multiple ordered sets, each with
any number of forms, and the author chooses how they are displayed. What the
user types is what gets stored. Clients that don't know the lexicon ignore it.

## Principles

- **Stored as typed.** No normalizing, reordering, merging or expanding forms.
- **No fixed shape.** A set is a list of forms. There is no predefined versus
  custom distinction, and clients never assume a set has a particular number
  of forms.
- **Author-controlled display.** `displayMode` says whether to show every set
  or only the first.
- **Self-sovereign.** One record (`rkey: self`) in the user's repo, read and
  written with the standard `getRecord` and `putRecord` calls. No external
  service.
- **Forward-compatible.** Unknown `displayMode` values fall back to `all`, so
  older clients keep working when new modes are added.

## Repository layout

```
atproto-pronouns/
  lexicons/app/nearhorizon/actor/pronouns.json   the lexicon
  docs/design/                                   this spec
  docs/images/                                   screenshots
```
