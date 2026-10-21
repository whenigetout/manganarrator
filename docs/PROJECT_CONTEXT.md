# manganarrator: project context

Source review: 2026-10-21. Baseline commit: `fd18e81933fb68c20c6320edb86e3440bf9d637a`.
For later sessions, compare relevant paths against that commit; avoid re-auditing unrelated services.

## Purpose and boundaries

Ecosystem documentation hub; no application server or package build lives here.

Route implementation to the owning service. Keep the ecosystem map here authoritative; link to service-specific details instead of copying them.

## Code map

README.md; docs/ECOSYSTEM.md

## Setup and checks

No application tests or launch command. Validate Markdown links and inspect `git diff --check`.

## Known details and gaps

The older README describes the original manga pipeline. Consult the ecosystem map for the independent Audio Studio and new Story Studio work.

## Current state / next work

This note was grounded in source inspection at the baseline above. Runtime verification for this service is recorded below only when actually executed. Documentation-only edits do not establish that GPU inference, external APIs, or the complete distributed manga pipeline work in this environment.

For story production, first inspect [Story Studio in the video service](https://github.com/whenigetout/manganarrator_video/blob/main/docs/STORY_STUDIO.md). The first slice uses imported segment audio; OCR and Paddle are unchanged. Native story TTS is a later adapter, not an image-shaped workaround.

## Related repositories

[Central ecosystem map](https://github.com/whenigetout/manganarrator/blob/main/docs/ECOSYSTEM.md) owns service responsibilities and the cross-repo task routing table. Consult it only for work crossing a service boundary.
