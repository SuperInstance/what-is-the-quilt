# What is the Quilt?

> A one-page explainer for newcomers. Read this first, then drill into the linked artifacts.

The Quilt is a multi-repo project family that builds a **git substrate where the task is the atom, state is derived from the record, and provenance is the arbiter**. It is being shipped as nine public repos in three GitHub orgs, with a private R&D lab for the unverified work.

## The one-sentence version

> The Quilt is git projected as an agent substrate: every record you push is an event, every state is derived, and git itself is the database.

## The five things you need to know

### 1. The fold is the heart

A pure function `derive(records) → state`. Three ports (TypeScript, Python, Elixir) and one in progress (Rust) all pass the same 1,151-case conformance corpus with byte-identical digests. The fold is mechanical: class precedence is absolute (`override > task > witness`), within a class the most-derived candidate wins, remaining ties break by ancestor depth and id.

```ts
// Three ports. Same input. Same bytes out.
import { derive } from "@patchwork-quilt/git.pp-ng/fold"
import { fold } from "referee"  // Python
import { Gitpp.Fold } from "gitpp_beam"  // Elixir
```

### 2. The record is the truth

A record is `{id, parents, kind, effects, reason}` plus an optional `digest` claim. The runner writes the digest, the worker verifies it, the fold never reads it. Format v1 is first-class forever; migration to v2 is writers-first with no flag day.

### 3. Git is the only write

The fold doesn't trust any service in the write path. The runner refuses malformed records; the worker collects them; the fold derives the state. State is *never* stored, only ever derived. The cache is a derived artefact; the record is the truth.

### 4. Six ports, one algorithm (30/30 byte-for-byte equivalent)

The fold is implemented in:

- **TypeScript** ([`patchwork-quilt/git.pp-ng`](https://github.com/patchwork-quilt/git.pp-ng)) — the canonical port, the hub, in production
- **Python** (the referee, in `git.pp-ng/referee/`) — the portable reference implementation, the byte-level oracle
- **Elixir** ([`patchwork-quilt/gitpp-beam`](https://github.com/patchwork-quilt/gitpp-beam)) — GenServer per project, burn/restart-safe
- **JavaScript** ([`SuperInstance/gitpp-js`](https://github.com/SuperInstance/gitpp-js)) — 4th port, pure ESM, runs in any modern browser or Node 18+
- **C# / .NET 9** ([`SuperInstance/gitpp-cs`](https://github.com/SuperInstance/gitpp-cs)) — 5th port, stdlib only, AOT-compilable, Azure-Functions-friendly
- **Perl 5** ([`SuperInstance/gitpp-pl`](https://github.com/SuperInstance/gitpp-pl)) — 6th port, stdlib only, 4-byte UTF-8 handled natively (the rocket emoji 🚀)

Plus one scaffolded:

- **Rust** ([`SuperInstance/gitpp-rust`](https://github.com/SuperInstance/gitpp-rust)) — awaiting cargo in a future session

All 5 active ports (TS, Python, Elixir, JS, C#, Perl) follow the same **doc-only discipline**: written from the public spec, not from each other's source. A port that reproduces the pinned digests from the spec alone proves the spec is complete.

The cross-port artifact at [SuperInstance/gitpp-cross-port](https://github.com/SuperInstance/gitpp-cross-port) runs the same 30 cases through the 4 doc-only ports (Python, JS, C#, Perl) and proves they produce byte-for-byte identical closure roots. **30/30 cases agree. 4/4 pinned leaves match. 0 disagreements.**

The cross-port byte-equivalence is the strongest possible conformance test: if two ports written from prose alone agree on every byte of every leaf, the spec is normative, not a tutorial.

### 5. The corpus is the receipt

A conformant port is verified by `cargo test --release` (or `npm test`, or `mix test`) reproducing:

- The pinned closure root for the live `poc-quilt-1@task-c` project: `60effe23970d8a5cc837636c44685502361288c1f00577e9addfa10cbd4fc91d`
- 1,151/1,151 cases identical across ports
- 16,290 closure-root digests identical across ports

If the bytes don't match, the port has drifted. The debug path is the leaf domain: compute `record_leaf(record)` for one known record in your port and in the hub's referee; if the bytes differ, the canonical serializer is wrong.

## The repos

| Org | Repo | What |
|---|---|---|
| `patchwork-quilt` | [`git.pp-ng`](https://github.com/patchwork-quilt/git.pp-ng) | **The hub.** TypeScript, Cloudflare Workers + Artifacts. The canonical port, the conformance corpus, the Worker, the runner. |
| `patchwork-quilt` | [`git.pp`](https://github.com/patchwork-quilt/git.pp) | The original shell + Python substrate. The audit stream, region gate, ledger, ship-gate. |
| `patchwork-quilt` | [`git.pp-grid`](https://github.com/patchwork-quilt/git.pp-grid) | The Grid: a shared month derived from a default and an append-only record. |
| `patchwork-quilt` | [`git.pp-desk`](https://github.com/patchwork-quilt/git.pp-desk) | The captain's desk: a Cloudflare Agents projection of the record. |
| `patchwork-quilt` | [`gitpp-beam`](https://github.com/patchwork-quilt/gitpp-beam) | The Elixir port. |
| `patchwork-quilt` | `cloudflare-next-git-rd` (private) | R&D lab for the Cloudflare Build-the-Next-Git competition. |
| `SuperInstance` | [`quilt`](https://github.com/SuperInstance/quilt) | The published npm monorepo (cli, core, mcp, sdk, tui). |
| `SuperInstance` | [`quilt-cloudflare`](https://github.com/SuperInstance/quilt-cloudflare) | A Quilt reactive runtime on Cloudflare Workers + D1 + Vectorize + KV + R2. |
| `SuperInstance` | [`quilt-pincher`](https://github.com/SuperInstance/quilt-pincher) | A reflex engine built from Quilt cells; federates cloud / workstation / ESP32. |
| `SuperInstance` | [`quilt-doctor`](https://github.com/SuperInstance/quilt-doctor) | Fleet-health diagnosis (JEPA, MOTH, SPECTRAL, JEV lenses). |
| `SuperInstance` | [`git.pp-doctor`](https://github.com/SuperInstance/git.pp-doctor) | Record-set validator (Python, stdlib only, byte-identical to the hub's referee). |
| `SuperInstance` | [`git.pp-spec`](https://github.com/SuperInstance/git.pp-spec) | The canonical, normative spec. |
| `SuperInstance` | [`gitpp-rust`](https://github.com/SuperInstance/gitpp-rust) | The Rust port. In progress. |
| `SuperInstance` | [`quilt-research-canons`](https://github.com/SuperInstance/quilt-research-canons) | The research canon: scout series (53+ reports), sprint lineage, the §6 doctrine doc. |
| `SuperInstance` | [`quilt-status`](https://github.com/SuperInstance/quilt-status) | A single-page index of the 4 orgs and 9 active repos. |
| `SuperInstance` | [`fold-bout`](https://github.com/SuperInstance/fold-bout) | A judged head-to-head: MiniMax vs Claude Code implement the deterministic fold. |
| `purplepincher` | [`deckboss`](https://github.com/purplepincher/deckboss) | The flagship product. Voice-first, offline-first fishing logbook. |
| `si-ready` | (empty) | The curated shelf for ready-to-use applications. |

## The one thing to try first

```bash
# 1. The hub has a public read-only API at gitpp.superinstance.dev
curl -sS https://gitpp.superinstance.dev/api/records?project=poc-quilt-1 | jq '.records | length'
# -> 8

# 2. The 8 records of poc-quilt-1 reproduce the pinned closure root
curl -sS https://gitpp.superinstance.dev/api/records?project=poc-quilt-1 \
  | jq '.records' \
  | python3 -c "import sys, json; print('record count:', len(json.load(sys.stdin)))"

# 3. The validator that confirms the closure root, in one command
pip install --user 'git+https://github.com/SuperInstance/git.pp-doctor'
curl -sS https://gitpp.superinstance.dev/api/records?project=poc-quilt-1 \
  | jq '.records' \
  | python3 -m gitpp_doctor root - --at task-c
# -> "60effe23970d8a5cc837636c44685502361288c1f00577e9addfa10cbd4fc91d"
```

If you got that hash, you just confirmed that the spec and the live deployment agree, in three lines, with a tool that has no dependencies.

## The design philosophy

Two laws, from `git.pp/ARCHITECTURE.md`:

1. **Git is the system of record, and the push is the only act.** Anything outside git is a cache rebuildable from the repo, a content-addressed blob the repo points to, or ephemeral and receipted by a commit.
2. **Nothing is graded on evidence it produced or selected.** An agent does not grade its own drafts; a judge does not certify its own confidence; a gate does not learn only from outcomes it chose to see. Every grade comes from evidence something else produced, chosen by a mechanism the graded party could not predict.

The first law makes the system durable and auditable. The second makes it honest. The Quilt's audit stream (keyed-hash blind selection), region gate (per-region per-verdict conformal bound), ledger (selection-propensity-weighted Brier), and ship-gate (per-region churn, polarization monitor) are the machinery for the second law.

## What the Quilt is not

- **Not a database.** A database stores state. The Quilt derives state from a record.
- **Not a chat.** The Quilt is a substrate for any agent that can write a record; the chat interface is one of many possible views.
- **Not a wrapper around git.** The Quilt projects git as a substrate — every record is a commit, every derived state is a fold, but the underlying git operations are unchanged. You can `git log` the history.
- **Not a blockchain.** A blockchain distributes trust; the Quilt concentrates it in the record and the fold.
- **Not done.** The §6 doctrine question is open (see below); the Rust port is in progress; the audit/gate/ledger/ship-gate stack needs broader validation; the public deployment is one project with 8 records.

## The open design questions

- **§6 doctrine (PR #16):** May a third party's accusation, once ratified, stain a record its author still stands behind? Three options are documented in [`quilt-research-canons/research/retraction-doctrine-2026-10-15/DOCTRINE.md`](https://github.com/SuperInstance/quilt-research-canons/tree/main/research/retraction-doctrine-2026-10-15); the recommended answer is Option II (captain-retract, no stain). The captain's call.
- **Verification model (1c in `OPEN-QUESTIONS.md`):** self-consistency-only inside the fold, or a trusted service in the write path? The latter re-introduces a service the design deliberately avoided. The captain's call.
- **Lock-vs-override (1d in `OPEN-QUESTIONS.md`):** are scoped defaults a class below task (option A) or a mandate tier above override (option B)? The captain's call.

These are real design questions, not bugs. The Quilt's design discipline surfaces them explicitly; the answers are policy decisions, not engineering ones.

## What to read next

- **If you want to use the system:** start with `git.pp-ng/README.md` and the live API at `https://gitpp.superinstance.dev/?project=poc-quilt-1`.
- **If you want to port the fold to a new language:** start with [`SuperInstance/git.pp-spec`](https://github.com/SuperInstance/git.pp-spec) and follow the doc-only-discipline workflow in `PORTS.md`.
- **If you want to read the design:** start with `git.pp/ARCHITECTURE.md` (the two laws) and `git.pp-ng/DESIGN.md` (the algorithm).
- **If you want to follow the R&D:** start with [`quilt-research-canons`](https://github.com/SuperInstance/quilt-research-canons), specifically the most recent scout report and the `retraction-doctrine-2026-10-15/` directory.
- **If you want to evaluate a port:** start with the receipt (the pinned closure root) and a 1,151-case corpus run.

