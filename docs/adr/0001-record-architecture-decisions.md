---
status: accepted
date: 2026-11-16
---

# 1. Record architecture decisions

## Context

Kestra explains some of its design in `docs/architecture/`: the metric conventions and the plugin auto-install
design. Those pages are long and mix the decision with how it's built, so a contributor can miss what a change must
keep, such as a Counter's `.total` suffix or the editions where auto-install stays off.

## Decision

Record each architecture decision as a short Markdown file in `docs/adr/`, numbered in order, in the MADR format:
front matter with `status` and `date`, then Context, Decision and Consequences.

The first decisions recorded (0002 and 0003) are ones Kestra already made and documents in `docs/architecture/`.
Each names the page it comes from, which stays the full design.

## Consequences

- The constraints a change must keep are listed in one folder, each with its reason.
- A decision that changes gets a new ADR that supersedes the old one, and the old one stays, marked superseded.
- The pages in `docs/architecture/` stay the detailed design. An ADR states the decision and its cost, and links
  there.
