# Rimay

> Source code is private. This page documents the project for review; happy to share full read access on request.

*Rimay* means "to speak" in Quechua. It is a local Mac companion agent: it sees the screen, talks
back with a voice, and acts on macOS apps and the web behind an explicit confirmation step. A cuy
(guinea pig) mascot animated on the desktop is its face.

## What it does

- **Sees**: takes an accessibility-tree snapshot of the frontmost macOS app, or an indexed DOM
  snapshot of a web page, instead of reasoning over raw pixels.
- **Acts**: clicks, types, scrolls, opens apps, navigates URLs, on both macOS apps and web pages
  through the same small vocabulary of tools. Every element is addressed by an index from the
  snapshot, then dispatched to the live accessibility handle, not by simulated mouse coordinates.
  This keeps your own mouse cursor untouched while it works.
- **Confirms before acting**: state-changing actions go through an explicit confirmation gate
  rather than running unattended.
- **Talks**: local text-to-speech (Kokoro) and speech-to-text (Whisper), phrase by phrase, so it
  can hold a spoken conversation while it works.
- **Has a face**: a WebGL mesh-rig cuy avatar (desktop overlay window) reacts to speech amplitude
  and state, with a lighter sprite-based fallback renderer.

## Stack

- Python 3.11, project-local virtualenv.
- Claude (Anthropic SDK) as the agent loop: it holds real tools and sees each tool's result before
  deciding the next action, instead of following a pre-planned static script.
- Playwright over the Chrome DevTools Protocol for the web hand, against a Chrome instance
  dedicated to the agent (separate profile and port from the user's own browser).
- macOS Accessibility APIs for the native hand (reading and acting on other apps' UI trees).
- Kokoro (TTS) and Whisper (STT), both running locally.
- Ollama, optional, for local-model fallback in the conversation pipeline.
- The mascot: WebGL mesh rig for the primary render, a sprite-based renderer as a second, lighter
  option.

## Positioning

100% local by default: speech, vision, and the agent loop can all run without a network call.
Cloud models (Claude, optionally Gemini) are used for the agent's reasoning and are the one
optional dependency on a remote API.

This repository is the agent itself, not a packaged product. An earlier commercial line built on
top of it, a WhatsApp receptionist sold to clinics, has been extracted into a separate repository.
What's left here is the companion agent: the loop, the two hands (macOS and web), the voice
pipeline, and the mascot.

## Status

Active, personal project. Not a finished or sold product, and not currently packaged for anyone
else to install and run out of the box: paths, launch scripts, and some configuration assume the
author's own machine. Treat this as a portfolio piece showing the architecture and the approach,
not as a ready-to-deploy tool.

The internal test suite lives in `core/test_all.py`. It exercises macOS accessibility and audio
paths, so it needs to run on macOS with accessibility permissions granted, and hasn't been
re-verified for this snapshot of the repo.

## Layout

- `core/`: the agent loop (`boucle.py`), conversation orchestration, TTS/STT wiring, and the
  internal test suite.
- `agent/`: the web hand (indexed DOM snapshot and actions over CDP/Playwright).
- `avatar/`: the desktop mascot: the WebGL mesh rig, the sprite renderer, and the tooling used to
  generate their assets.
- `brain/`: persistent memory and knowledge sources the agent reads from and writes to.
- `site/`: small web surfaces (demo pages, the mascot's browser-facing assets).
- `ops/`: deployment and operational scripts, append-only audit log.
- `stt/`: the speech-to-text binary and its build script.
