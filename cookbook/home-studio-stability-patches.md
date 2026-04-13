# Home Studio Stability Patches

This fork carries the Hindsight API changes that were validated on the Home Studio Mac Studio after the stock runtime showed instability under Paperclip-heavy workloads.

## Included in this fork

- `hindsight-api-slim/hindsight_api/engine/retain/chunk_storage.py`
  - Retain chunk writes are now idempotent via `ON CONFLICT (chunk_id) DO UPDATE`.
  - This prevents repeated retains of the same document from crashing with `pk_chunks` unique-key violations.

- `hindsight-api-slim/hindsight_api/worker/poller.py`
  - Pending non-consolidation tasks prioritize `paperclip::` banks before generic repo banks.
  - This keeps Paperclip recall/retain traffic responsive even when large background repo backfills are queued.

## Still maintained outside this fork

The Home Studio deployment also depends on companion runtime patches that live outside the Hindsight repository:

- Paperclip worker checkout/self-conflict handling
- Paperclip OpenClaw gateway compatibility shim
- Memory Broker `ulimit -n 65536` startup hardening
- Local verification harness updates for end-to-end Paperclip/Hindsight validation

Those changes should be versioned in their own repositories or deployment overlays if you want the whole stack to be reproducible from source.
