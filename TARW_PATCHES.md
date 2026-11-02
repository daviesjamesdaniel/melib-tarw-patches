# melib-tarw-patches

A `git subtree split` of the `melib/` directory of
[meli](https://git.meli-email.org/meli/meli) at **v0.9.0**, with a small
stack of patches [tarw](https://github.com/daviesjamesdaniel/tarw) needs that
aren't in a meli release yet. Each patch is dropped once it lands upstream, so
this fork should shrink over time, not grow.

## Patches on top of v0.9.0

| Patch | Why | Upstream |
|---|---|---|
| Inline workspace-inherited dependency versions and lints | A standalone subtree has no workspace root, so `workspace = true` can't resolve | Not applicable (fork-only) |
| error: classify UnexpectedEof as a dead connection | Lets callers reconnect instead of surfacing a generic error | Not upstream yet |
| imap/connection: tag a real BYE response as a dead connection | Same, for a server-initiated BYE | Not upstream yet |
| imap: retry with backoff if a mailbox isn't yet in the map on fetch() | Avoids a startup race between mailbox listing and the first fetch | Not upstream yet |
| imap/sync: avoid forced re-SELECT in resync_basic/resync_condstore | Saves a round trip on every background resync | [#728](https://git.meli-email.org/meli/meli/pulls/728) (draft) |
| imap: cache the mailbox list, prune deleted mailboxes, fix metadata bugs | Mailbox list available at launch without a LIST round trip | [Issue #706](https://git.meli-email.org/meli/meli/issues/706) |
| imap/operations: always peek when fetching a message body | Fetching a body must never mark it read as a side effect | Not upstream yet |
| imap/sync: replace removed to_str! macro in mailbox-list cache | Adapts the mailbox-list cache to v0.9.0 | Fork-only adaptation |
| imap/sync: only wipe envelope cache on a genuine UIDVALIDITY change | Stops the envelope cache being wiped when a mailbox row is merely missing | Not upstream yet |
| imap/sync: treat mailbox rows without a UIDVALIDITY as not yet cached | Mailbox-list placeholder rows broke on v0.9.0's NonZero UIDVALIDITY | Fork-only adaptation |

## Not carried over from the v0.8.13-based fork

- **imap/fetch: use UID SEARCH ALL for the initial full sync** (and its
  regression test): being reworked for v0.9.0 as
  [#759](https://git.meli-email.org/meli/meli/pulls/759); re-added once done.
- **imap: don't build an invalid empty range when a mailbox is actually
  empty**: v0.9.0's reworked EXISTS handling appears to cover it; pending a
  live check (delete the last message in a folder).

## Testing

`cargo test` with every feature except `sequoia` (which needs `capnp` and a
nettle with `pgp.h`, removed in nettle 4.0).
