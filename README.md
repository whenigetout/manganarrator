# MangaNarrator

MangaNarrator is a distributed AI pipeline that transforms static manga or manhwa panels into structured, emotion-aware narrated video content.

It integrates OCR, emotion tagging, TTS synthesis, and deterministic video rendering into a modular, service-oriented architecture.

---

## 🚀 Overview

Manual narration of manga content is time-consuming and difficult to scale.

MangaNarrator solves this by automating the full pipeline:

1. Extract dialogue and metadata from images using OCR
2. Propagate emotion labels and character information
3. Generate emotion-aware voice synthesis
4. Render synchronized video output using FFmpeg
5. Support batch processing via structured run IDs

The system is designed for scalability, traceability, and local GPU inference.

---

## 🏗 System Architecture
```
[Input Images]
↓
[OCR Service]
↓
[Shared Contracts Layer]
↓
[TTS Service]
↓
[Video Builder]
↓
[Final Rendered Video]
```

Each component is isolated as an independent service.

---

## 🧩 Repositories for the independent services

- **[manganarrator_orchestrator](https://github.com/whenigetout/manganarrator_orchestrator)**  
  Django-based orchestration layer coordinating pipeline execution and I/O.

- **[manganarrator_frontend](https://github.com/whenigetout/manganarrator_frontend)**  
  TypeScript frontend for managing runs, previewing OCR output, and triggering TTS/video builds.

- **[manganarrator_ocr](https://github.com/whenigetout/manganarrator_ocr)**  
  Vision-based OCR service extracting dialogue, bounding boxes, speaker metadata, and emotion labels.

- **[manganarrator_tts](https://github.com/whenigetout/manganarrator_tts)**  
  Emotion-aware text-to-speech engine with configurable inference parameters.

- **[manganarrator_video](https://github.com/whenigetout/manganarrator_video)**  
  FFmpeg-driven deterministic video assembly module.

- **[manganarrator_contracts](https://github.com/whenigetout/manganarrator_contracts)**  
  Shared schemas and models used across services for structured data exchange.

- **[manganarrator_paddle_ocr](https://github.com/whenigetout/manganarrator_paddle_ocr)**  
  PaddleOCR-based helper module for alternate OCR backend support.

---

## ⚙️ Engineering Highlights

- UUID-based image traceability across the pipeline
- Run ID grouping for large batch processing
- Structured OCR JSON with dialogue-level IDs
- Emotion propagation from OCR → TTS
- Deterministic FFmpeg command construction
- Modular service isolation for scalability
- Local GPU inference optimized for 8GB VRAM environments

---

## 🛠 Tech Stack

- Python
- Django
- TypeScript
- FFmpeg
- Vision/OCR models
- Emotion-aware TTS models
- Local AI inference workflows

---

## 🎯 Design Goals

- End-to-end automation
- Modular service architecture
- Scalable batch processing
- High traceability for debugging
- Extensible for future AI modules

---

## 🔮 Roadmap

- Distributed worker queue support
- Advanced timeline editor in frontend
- Multi-speaker emotion control
- Cloud-deployable inference option
- Performance benchmarking dashboard

---

## 📌 Status

Active development. Designed as a scalable foundation for AI-driven media automation.
