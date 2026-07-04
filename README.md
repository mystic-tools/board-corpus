# MYSTIC Board Corpus

Public getting-started knowledge for embedded boards (Doc 62 D1).

```
<chip>/_chip/       shared chip reference (sparse-checkout friendly)
<chip>/<board>/     board overlay — wins on filename collision
```

**PUBLIC tier:** chip + board getting-started context only.
**PRIVATE tier (never here):** verified fixes, application macros, customer notes.

End users pull via `pack install` (unauthenticated read) or sparse clone.
