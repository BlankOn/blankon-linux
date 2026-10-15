# Request For Comment: BlankOn Linux as a Guarded Rolling Release Against Debian Sid

- **Status:** Draft
- **Author:** Herpiko Dwi Aguno
- **Date:** 2026-09-04
- **Target audience:** BlankOn Linux maintainers, packaging/infra team

## 1. Summary

This RFC proposes changing BlankOn Linux's release model from periodic, versioned releases to a **guarded rolling release** tracking Debian Sid. "Guarded" means BlankOn Linux does not expose Sid directly to users: every pull from Sid lands first in a staging repository, goes through manual QA performed by human contributors, and is only promoted to the public repository once it is judged stable.

Anything that fails QA is patched through BlankOn's existing `IRGSH` build system before being retried, and every staging update is wrapped in a Btrfs snapshot so a bad pull can be rolled back without a rebuild.

The goal is to keep BlankOn closer to upstream Debian (fresher packages, less packaging drift, less end-of-life pressure from doing full version-to-version migrations) while keeping the actual user-facing repository as stable as a point release.

## 2. Motivation

BlankOn currently packages and QAs against a point-in-time Debian base, which creates a few recurring problems:

- Conventional releases are costly for us because we have to manage multiple repositories for multiple releases, both in terms of effort/time and infrastructure.
- Packaging drift accumulates between BlankOn releases, and each new release effectively re-does a large rebase against a much newer Debian, which is a big, risky, all-at-once migration.
- Security and bug fixes that land in Debian have to wait for the next planned release cycle to reach BlankOn users, unless they are manually backported.
- Contributor sustainability remains a challenge. Contributors often leave after a major release, making it difficult to maintain the project's momentum and sustainability.
- Major releases are often postponed or hindered because everyone strives for perfection, which is ultimately unattainable.

## 3. Goals

- Set new added value for the distribution, which is "Guarded Rolling Release of Sid"
- Make the Linux distribution more manageable and sustainable, keeping the process cost-effective and feasible.

## 4. Current State (for reference)

- Package repositories are managed with `reprepro`, currently serving the `verbeek` distribution, multi-arch (amd64 and arm64).
- Builds go through the `irgsh` build system (pbuilder-based).
- ISOs are produced with `live-build` and cron-scheduled daily builds.
- The public archive is served from `arsip.blankonlinux.id` while the development side served from `arsip-dev.blankonlinux.id`

This RFC keeps all of the above in place and inserts a staging/QA/gating stage in front of the existing publish step, rather than replacing the toolchain. The one exception is the distribution codename itself: see section 7.

## 5. Proposed Pipeline

```
   Debian Sid
       |
       | scheduled pull (weekly or monthly)
       v
 +------------------------------+
 | arsip-dev (staging repo)     |<--------------------+
 | reprepro distribution,       |                      |
 | Btrfs snapshot before pull   |                      | rollback to
 +---------------+--------------+                      | previous snapshot
                  |                                    |
                  v                                    | 
           +-------------+                             |
           |  QA works   |  (manual)                   |
           +------+------+                             |
                  |                                    |
                  v                                    |
            +-----------+   unstable, needs   +------------+
            |  Result   |----patching-------->|  Patch via |
            |  (gate)   |                     |    IRGSH   |
            +-----+-----+                     +------+-----+
                  |  all clear                        |
                  |  promote to users                 |
                  v                                   | re-submit to
     +--------------------------+                     | arsip-dev
     | arsip.blankonlinux.id    |<--------------------+
     | dist: blankon (public)   |
     +-------------+-------------+
                    |
                    v
                  Users
```

This is a direct implementation of the flow already sketched out informally: Sid pulls into `arsip-dev.blankonlinux.id`, manual QA by human contributors, a gate that either promotes to `arsip.blankonlinux.id` or routes packages through the IRGSH patch loop, with a rollback path to the previous Btrfs snapshot when a pull is too broadly unstable to patch forward.

This process must be performed manually by a team member, based on an agreement among the team, before proceeding with the update.

### 5.1 Take snapshot of `arsip-dev`

Before pulling any updates from Sid, we must take a Btrfs snapshot of the repository partition. This ensures that we can roll back if we receive a catastrophic update.

### 5.2 Pull stage (Sid to `arsip-dev`)

After the snapshot is secured, we will pull updates from Sid.

### 5.3 QA stage

QA runs manually against `arsip-dev.blankonlinux` after each pull, performed by human contributors. There is no automated QA in this design; every pull is reviewed by a person before it can be promoted.

Because this work sits directly on the critical path of every pull, the size and sustainability of the QA contributor pool matters for keeping the pipeline healthy. See section 10 for how this RFC proposes growing that pool.

### 5.4 Gate ("Result")

The gate has three possible outcomes per pull, decided by the human contributors who ran QA:

1. **All clear, promote.** The `arsip-dev.blankonlinux.id` snapshot is synced/promoted into `arsip.blankonlinux.id`, and becomes what existing users' `apt upgrade` will pull.
2. **Some packages unstable, patch via IRGSH.** Specific failing packages are identified, patched, and rebuilt through the existing `irgsh` build system, then re-submitted into `arsip-dev` for another QA pass. This is a targeted loop: only the failing packages are touched, not the whole pull.
3. **Broadly unstable, rollback.** If QA turns up problems too broad or too deep to patch in a reasonable window, `arsip-dev` is rolled back to the pre-pull Btrfs snapshot and the team waits for Sid to stabilize before re-attempting the pull. This is the safety valve that keeps a bad upstream moment from consuming the team's patching bandwidth.

### 5.5 Promotion (`arsip-dev.blankonlinux.id` to `arsip.blankonlinux.id`)

Promotion is a `reprepro` operation (copy/mirror between distributions), not a rebuild.

After a promotion, we can announce the new update to users and let them choose whether to update their systems.

## 6. Infrastructure Changes Required

| Area | Change |
|---|---|
| `reprepro` distribution naming | Drop the `verbeek` codename. The production distribution served from `arsip.blankonlinux.id` will simply be named `blankon` going forward. |
| `reprepro` | Add `arsip-dev` staging distribution alongside the (renamed) `blankon` production distribution; define promotion tooling/scripts between `arsip-dev` and `blankon`. |
| Storage | `arsip-dev`'s backing filesystem must be Btrfs (or the staging repo moved onto a Btrfs-backed volume) to support pre-pull snapshot/rollback. |
| Scheduling | Disable all cron jobs that automatically pull updates from Sid, as updates must be decided through team discussion and agreement. |
| QA process | Define and document the manual QA checklist and workflow for human contributors, and how QA sign-off is recorded so the gate decision (section 6.3) has a clear, auditable trigger. |
| `irgsh` | Extend/confirm `irgsh` can take a targeted "patch this package and resubmit to `arsip-dev`" job rather than only full builds. |
| Monitoring | Alerting on: pull failures, QA sign-off status, gate decisions, and successful/failed promotions, so the team knows when manual intervention (patch or rollback) is needed. |
| Documentation | Update contributor docs to describe the new `arsip-dev` to QA and produce a QA contributor guide/checklist to support onboarding new QA contributors, including interns (see section 9). |

## 7. Versioning and User Communication

Users should be told, at minimum: what "rolling" means for their upgrade cadence, that BlankOn is Sid-derived-but-gated (not raw Sid), and how to report regressions that make it through the gate (manual QA reduces but does not eliminate the chance of user-visible bugs).

Since we don't have any existing point-release users at the moment (because of the long dormant), we don't need a documented migration path onto the rolling track.

## 8. Risks and (Mitigations?)

- QA gaps let a regression through.
- Storage growth from Btrfs snapshots.
- QA becomes a bottleneck as a purely manual process, or Sid instability outpaces the team's patching capacity

## 9. Impact to Foundation Work

Making QA a continuous part of every pull creates an opportunity for BlankOn’s foundation to establish a dedicated QA internship track, giving newcomers practical distribution QA experience while turning recurring QA work into a structured community contribution. This should begin once the manual QA process and contributor guide are stable.

## 10. Rollout Plan

1. Document the manual QA checklist and contributor workflow.
2. Stand up `arsip-dev` as a new `reprepro` distribution on Btrfs-backed storage, and rename the production distribution from `verbeek` to `blankon`; verify snapshot/rollback works in isolation.
3. Decide on the dist name, then execute a clean pull from Sid, then freeze at quite stable point.
4. Focus to relase a stable ISO as soon as possible. Wire up the scheduled pull job at a conservative cadence, maybe monthly.
5. Try to deliver stable upate to users of no. 4
6. Improve the workflow overtime.

## 11. Open Questions for Discussion

- What is the concrete manual QA checklist (exact steps and sign-off criteria) beyond the categories listed in section 5.2? What tool we use to ease the QA works.
  - Improve packages.blankonlinux.idto generate check list based on the diff.
- Weekly vs. monthly pull cadence: decide based on rollout step 6, or set by team consensus from time to time?
  - Somat: maybe 2 weeks, if we wait too long, the potential bugs will be pilled up.
    - We will wait more if the updates
  - We need to improvte packages.blankonlinux.id to mmake it able to deliver summary to us
- What is the scope, timeline, and mentorship structure for the QA internship track proposed in section 9?
- Do we need an additional backup of `arsip-dev` on another server? This would ensure that, even if we cannot roll back, we still have a backup that allows us to continue the project. `arsip.blankonlinux.id` will be cloned/backed up by mirrors, but this is essentially a one-way backup and cannot be managed or have packages injected into it again by `reprepro`.
- How to measure it is stable or not? What are the metrics? How to gain the concencus?
- What is the dist name for this rolling release scheme? Just `blankon` or other specific name like Tumbleweed? Some options:
  - BlankOn Sinambung
  - BlankOn Gulir
  - BlankOn Satata
  - BlankOn Saban
  - ?
- If regression happened on the users side, we need to provide them the most feasible way to report a bug and QA team should have feasible way too to triage the bug tickets. What platform we can use for this?
  - Spawn instance forum (bapak yang dari solydxk mau bantu moderasi)
  - Formulir web sederhana untuk lapor kutu
  - Simple GNOME app to report bug

<img width="1040" height="691" alt="gambar" src="https://github.com/user-attachments/assets/2cf7ea81-ed71-46fa-8257-9ce02d3daf6f" />
