# MangaNarrator ecosystem map

Verified against repository source on 2026-10-21. Each repo's `docs/PROJECT_CONTEXT.md` records its review baseline. Start there and inspect relevant subsequent changes; do not reread all eight repos for routine work.

See [Story Studio architecture and priorities](STORY_STUDIO_ARCHITECTURE.md) for the implemented production slice and the next useful increments.

## Service ownership

| Repository | Owns | Starting points | Launch / integration |
| --- | --- | --- | --- |
| [manganarrator](https://github.com/whenigetout/manganarrator) | Overview and cross-repo decisions | This map, README | Documentation only |
| [manganarrator_frontend](https://github.com/whenigetout/manganarrator_frontend) | Manga workbench, OCR correction, dialogue/bbox editor | `app/(manga_narrator)/manga_narrator/`, `components/` | Next.js, `npm run dev`, port 4000 |
| [manganarrator_orchestrator](https://github.com/whenigetout/manganarrator_orchestrator) | Shared media filesystem API and OCR proxy | `manga_narrator/views.py`, `urls.py`, `utils.py` | Django, port 9000 |
| [manganarrator_ocr](https://github.com/whenigetout/manganarrator_ocr) | Vision dialogue extraction and bbox reconciliation | `ocr_server.py`, `app/ocr_runner.py`, `app/backends/` | FastAPI; orchestrator defaults to port 7860 |
| [manganarrator_paddle_ocr](https://github.com/whenigetout/manganarrator_paddle_ocr) | Text geometry helper | `paddleocr_server.py`, `models/` | FastAPI, documented port 6002 |
| [manganarrator_tts](https://github.com/whenigetout/manganarrator_tts) | Chatterbox speech, voice references, emotion mapping | `tts_server.py`, `app/tts_runner.py`, `app/services/tts_resolver.py` | FastAPI; match the frontend's configured TTS URL |
| [manganarrator_video](https://github.com/whenigetout/manganarrator_video) | Manga renderer; Audio Studio; Story Studio; publishing | `video_server.py`, `app/story/`, `app/audio_studio.py`, `frontend/src/`, `app/publishing/` | FastAPI 8084; React/Vite embedded at `/studio/` |
| [manganarrator_contracts](https://github.com/whenigetout/manganarrator_contracts) | Python, TypeScript and story JSON Schema | `python/mn_contracts/`, `ts/mn_contracts/`, `schemas/` | Git-installed library; regenerate TypeScript dist with source |

Ports are local defaults/examples, not service discovery. Verify local configuration before changing it.

## Three workflows

**Manga:** Next.js workbench → filesystem/orchestrator API and OCR → optional Paddle geometry → corrected dialogue → TTS → video preview geometry → FFmpeg render. Services share a media root with `inputs` and `outputs` namespaces.

**Audio Studio:** uploaded recording → measured audio/compositor → preview or full video → optional existing publishing workflow. It is hosted in the video repo, not in the Next.js manga frontend.

**Story Studio:** normalized Markdown/JSON → immutable source plus editable draft scenes → imported speech takes and still images → measured independent audio/visual tracks → selected scene preview or chapter MP4 plus manifest. It is additive in the video repo and bypasses OCR. [Usage and implemented scope](https://github.com/whenigetout/manganarrator_video/blob/main/docs/STORY_STUDIO.md).

## Route the task before opening code

| Task | Read first | Consult only if the boundary changes |
| --- | --- | --- |
| Story editing, revision history, image timing, chapter export | Video `app/story/`, `frontend/src/StoryStudio.jsx` | Contracts story module |
| New native story TTS adapter | TTS runner/backend + contracts | Video speech import API and stale-take rules |
| Audio spectrum, backgrounds, preview/export mismatch | Video audio compositor and tests | Embedded frontend settings |
| Manga dialogue editing or bbox placement | Manga frontend workbench | Contracts, OCR conversion, video preview |
| Missing shared files / traversal errors | Orchestrator path resolution and media settings | Relevant producing service |
| OCR extraction / detection mismatch | OCR model adapter and Paddle mapper | Paddle service |
| YouTube profiles, metadata, resumable upload | Video `app/publishing/`, publishing docs/tests | No other service normally needed |

## Decisions to preserve

- Two frontends currently serve different workflows. A redesigned story editor does not require replacing the manga frontend.
- Stories have stable hierarchy/segment IDs and no image bounding boxes. Do not fabricate `image_ref` values to satisfy the legacy TTS request model.
- First production slice accepts recorded or externally generated speech per segment. A native TTS adapter is future work.
- Final timing is measured from decoded audio. Word-count estimates are editorial planning only.
- Canon is separate from adaptation. Script/voice changes invalidate an old take; manual shot placements are preserved and conflicting coverage blocks export.
- Local FFmpeg rendering reuses the video backend. Assets, project revisions and manuscripts are private runtime data, not public Git fixtures.
- Existing publishing remains explicit. Rendering never posts a video.

## Keeping context useful

Every repo has a root `AGENTS.md` and a short `docs/PROJECT_CONTEXT.md`. Update affected notes when changing paths, commands, API boundaries, known gaps or verification. Record a date and exact check outcome. Keep future plans distinct from implemented behavior. Avoid copying private story text into handoffs. Do not turn the docs into an append-only conversation log.
