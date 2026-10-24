# The pronouns record

NSID: `app.nearhorizon.actor.pronouns`
Lexicon: [`lexicons/app/nearhorizon/actor/pronouns.json`](../../lexicons/app/nearhorizon/actor/pronouns.json)

## Record key

`literal:self`: one record per account, the same pattern as
`app.bsky.actor.profile`.

## Fields

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `sets` | `pronounSet[]` | yes | Ordered. The first entry is the primary set. Up to 10 sets. May be empty. |
| `displayMode` | string | no | Known values `all`, `firstOnly`. Absent or unrecognized means `all`. |
| `createdAt` | datetime | yes | Set once on first write. Never changed afterwards. |
| `updatedAt` | datetime | no | Set on every edit after the first write. Absent on first write. |

### `pronounSet`

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `forms` | string[] | yes | 1 to 12 forms, in the order the user gave them. Each form is up to 24 graphemes and 240 bytes, so 24 graphemes of CJK, Hangul or emoji fit. |

By convention `forms[0]` is the subject form and `forms[1]` the object form.
Clients must not assume a fixed number of forms.

## Normative rules

- **No normalization.** Clients MUST NOT normalize, reorder, merge, expand or
  otherwise alter forms when storing them.
- **`displayMode` default.** If `displayMode` is absent or has an unrecognized
  value, clients MUST treat it as `all`. Implement this fallback explicitly.
- **Empty sets.** `sets` may be an empty array. Clients SHOULD then show
  nothing (see [display.md](display.md)).
- **Timestamps.** `createdAt` MUST NOT change after the first write. Every edit
  preserves the original `createdAt` and sets `updatedAt` to the current time,
  including an edit that clears the sets.

## Clearing versus deleting

There are two ways to remove someone's pronouns, and they mean different
things to readers:

- **Clearing** writes the record with `sets: []`, keeping `createdAt` and
  setting `updatedAt`. This is an explicit "show no pronouns". It also stops
  clients from falling back to the Bluesky profile's `pronouns` field.
- **Deleting** the record with `deleteRecord` means the user has no record at
  all. Clients may then fall back to the Bluesky profile's field.

A client doesn't need to offer delete. Offering only "clear" is simpler, and
it reliably hides pronouns everywhere, including the fallback.

## Examples

Several sets, including neopronouns:

```json
{
  "$type": "app.nearhorizon.actor.pronouns",
  "sets": [
    { "forms": ["she", "her"] },
    { "forms": ["fae", "faer", "faerself"] },
    { "forms": ["star", "stars", "starself"] }
  ],
  "displayMode": "all",
  "createdAt": "2026-02-10T00:00:00Z"
}
```

A combined set, stored exactly as typed:

```json
{
  "$type": "app.nearhorizon.actor.pronouns",
  "sets": [{ "forms": ["she", "they · it", "faer"] }],
  "displayMode": "firstOnly",
  "createdAt": "2026-02-10T00:00:00Z",
  "updatedAt": "2026-03-01T12:00:00Z"
}
```

Cleared:

```json
{
  "$type": "app.nearhorizon.actor.pronouns",
  "sets": [],
  "createdAt": "2026-02-10T00:00:00Z",
  "updatedAt": "2026-04-02T09:30:00Z"
}
```

## Reading

No auth needed. Fetch the record from the user's PDS:

```bash
curl "https://bsky.social/xrpc/com.atproto.repo.getRecord?repo=did:plc:example&collection=app.nearhorizon.actor.pronouns&rkey=self"
```

## Writing

Standard `putRecord`, authenticated as the user. First write:

```typescript
await agent.com.atproto.repo.putRecord({
  repo: userDid,
  collection: 'app.nearhorizon.actor.pronouns',
  rkey: 'self',
  record: {
    $type: 'app.nearhorizon.actor.pronouns',
    sets: [
      { forms: ['she', 'her'] },
      { forms: ['they', 'them'] },
    ],
    displayMode: 'all',
    createdAt: new Date().toISOString(),
  },
})
```

Editing (and clearing) preserves `createdAt` and sets `updatedAt`:

```typescript
const existing = await agent.com.atproto.repo.getRecord({
  repo: userDid,
  collection: 'app.nearhorizon.actor.pronouns',
  rkey: 'self',
})

await agent.com.atproto.repo.putRecord({
  repo: userDid,
  collection: 'app.nearhorizon.actor.pronouns',
  rkey: 'self',
  record: {
    $type: 'app.nearhorizon.actor.pronouns',
    sets: [{ forms: ['they', 'them'] }],
    displayMode: 'all',
    createdAt: existing.data.value.createdAt, // preserve original
    updatedAt: new Date().toISOString(),       // set on every edit
  },
})
```
