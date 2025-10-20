# Recian

[![image](https://img.shields.io/pypi/v/recian.svg)](https://pypi.python.org/pypi/recian)
[![image](https://img.shields.io/pypi/l/recian.svg)](https://pypi.python.org/pypi/recian)
[![image](https://img.shields.io/pypi/pyversions/recian.svg)](https://pypi.python.org/pypi/recian)
[![Discord](https://img.shields.io/discord/1429860101090709579?logo=discord&style=flat)](https://discord.com/widget?id=1429860101090709579)
<!-- [![Actions status](https://github.com/synechic/recian/actions/workflows/ci.yml/badge.svg)](https://github.com/synechic/recian/actions) -->

![image](https://github.com/synechic/recian/blob/main/assets/banner.png?raw=true)

## About

**Recian** is a manifest-driven asset manager built for fast, deterministic
local access to remote content under real-world concurrency. **Instead of
probing files—or listing buckets and directories—the manifest is the source of
truth.** By using manifests there is **no need to list all local or remote
files**, which can be a costly delay during critical processing. All content
facts live in the manifest (with an optional “guarantee” mode), cutting file
checks, reducing validation churn, and enabling clean, differential updates.

## Definition

True to [Synechic](https://github.com/synechic)’s style, Recian is a coined name
shaped from venerable linguistic roots, deliberately blended into a modern,
memorable identity.

> Recian (ˈriːʃən, “REE-shun”) — adj.: relating to deliberate return or
> restoration; describing processes that regain or reestablish essential
> elements with clarity.

How to read the name
- re- — pronounced “ree” (IPA: /riː/) — think return, restore, retry: getting
  back to the right state efficiently.
- ci — pronounced “sh” (IPA: /ʃ/) — a cue toward clear knowledge and
  determinism: know what changed and why.
- -ian (here realized as -ən) — pronounced “uhn” (IPA: /ən/) — evokes the
  practiced, methodical feel of agentive endings (cf. technician), without the
  occupational sense.

The name attempts to express
- Repeatable recovery over blind rebuilds.
- Skilled, lock-aware handling rather than best-effort grabs.
- Transparent state: auditable diffs, declarative truth, verifiable results.

### What Recian focuses on

- **Manifest-first state:** A leveled catalog (root → child manifests) tracks
  what changed and where — so routine operations avoid extra listings and status
  calls. Manifests also carry **extended attributes** (e.g., **topics/tags**)
  used for targeted prefetch, rollout cohorts, and policy.
- **Stable placement, by design:**  Manifests are **assigned using hashing
  strategies** (for partitioning/sharding of *manifests*, not file validation).
  This keeps placement stable and **won’t rebalance unless explicitly forced**,
  preserving cache locality and predictable access paths.
- **Faster collection strategy:** Workflows **collect a single root manifest**,
  then **bulk-fetch** and **on-demand fetch** child manifests as needed. During
  preparation and normal operation, a **full desired file list** is evaluated to
  decide **which child manifests to fetch on demand**, which is **theoretically
  much faster** than broad remote walks.
- **Lock-aware realization:** Standard APIs and **context managers** expose
  **shared (read)** and **exclusive** locks, ensuring assets are available when
  acquired and never handed out in expired states. When limits are reached, work
  is **deferred** rather than thrashed.
- **Bounded cache pool:** Assets are staged in a constrained local pool.
  Eviction and admission are **lock-aware**, so readers aren’t disrupted and
  writers don’t race. Hard limits are respected without silent degradation.
- **Zero-copy working dirs:** Assets are isolated per task via **hard links**
  (with safe fallbacks), giving near-instant setup for function calls while
  preserving read-only integrity.
- **Pluggable coordination & storage:** Multiple lock and pool backends —
  **local file locks**, **AWS DynamoDB**, and **S3-based** coordination — plus
  providers for **local paths**, **generic HTTP(S)**, **AWS S3**, and
  **DynamoDB-backed catalogs**. All through a uniform contract.

### Why this shape works

- **Deterministic by design:** the manifest drives decisions; the filesystem
  follows.
- **Efficient at scale:** fewer remote checks, fewer redundant listings, **no
  directory/bucket walks**, and **targeted manifest fetches**.
- **Safe under load:** shared/exclusive locks, context-managed access, and
  defer-instead-of-thrash behavior keep systems stable when resources tighten.

### In short

Recian synchronizes and stages local assets—on demand or ahead of time—using
safe, standard-API locks and a bounded cache pool, with **manifest-carried truth
and attributes** guiding fast, predictable acquisition without revalidating
everything.

## Command Line Interface

The current command line is written in Python (aiming for Rust in a new major
version) and is available through pip, pipx, uvx by installing the `recian-cli`

program along with various extras.  The `recian-cli`
[README.md](https://github.com/synechic/recian/blob/main/python/packages/recian-cli/README.md)
goes over the various extras.

```shell
pip install recian-cli
```
