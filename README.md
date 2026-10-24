# ATProto Pronouns Lexicon

**Namespace:** `app.nearhorizon.actor.pronouns`
**Version:** 1.0
**License:** Apache 2.0

Pronouns as an ATProto record on the user's PDS. Not a labeler, not a single
free-form string, not someone else's service that can vanish. Multiple ordered
sets, any forms the user types, and author-controlled display.

<img src="docs/images/profile_feed.png" width="600" />

## The record

| NSID | Key | Holds |
| --- | --- | --- |
| `app.nearhorizon.actor.pronouns` | `self` | Up to 10 ordered pronoun sets (each 1 to 12 free-text forms), a `displayMode` (`all` or `firstOnly`), and timestamps. |

```json
{
  "$type": "app.nearhorizon.actor.pronouns",
  "sets": [
    { "forms": ["she", "her"] },
    { "forms": ["fae", "faer", "faerself"] }
  ],
  "displayMode": "all",
  "createdAt": "2026-02-10T00:00:00Z"
}
```

Display a set by joining its forms with `/`: `she/her · fae/faer/faerself`.

## Adopting it

1. Use the lexicon: [`lexicons/app/nearhorizon/actor/pronouns.json`](lexicons/app/nearhorizon/actor/pronouns.json).
2. Join each set's `forms` with `/`.
3. Follow `displayMode`, treating absent or unknown values as `all`.
4. Never normalize, reorder, merge or expand what the user typed.
5. Show nothing for an empty `sets` or a record that hasn't loaded yet.

## Docs

- [Overview](docs/design/overview.md): why it exists and the principles.
- [The record](docs/design/record.md): fields, limits, rules, clearing versus deleting, examples.
- [Display](docs/design/display.md): rendering, display modes, fallback, placement.
- [Changelog](CHANGELOG.md)

## Works alongside atproto-plural (optional)

[atproto-plural](https://github.com/skydeval/atproto-plural) describes plural
systems and their members, and its member pronouns use a similar shape. Each
lexicon stands alone: a client can adopt either one without the other.

## Implementations

- [Speakeasy Social](https://spkeasy.social): cooperative, privacy-focused social platform

Using this lexicon in your project? Open an issue or PR to get listed.

## License

Apache 2.0
