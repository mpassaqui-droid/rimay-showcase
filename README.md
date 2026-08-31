# Rimay

> Source code is private. This page documents the project at a high level; happy to share full read access on request.

*Rimay* means "to speak" in Quechua. It is a local Mac companion agent: it sees the screen, talks
back with a voice, and acts on macOS apps and the web behind an explicit confirmation step. A cuy
(guinea pig) mascot animated on the desktop is its face.

## What it does

Sees the screen (native app and web), acts on it through a small set of tools with a confirmation
step before anything state-changing, holds a spoken conversation via local text-to-speech and
speech-to-text, and shows a face: a WebGL mascot reacting to speech and state.

## Stack

Python, the Anthropic SDK as the agent loop, Playwright/CDP for the web hand, macOS Accessibility
APIs for the native hand, Kokoro (TTS) and Whisper (STT) running locally.

## Status

Active personal project, not a packaged product. Portfolio piece showing the architecture and
approach, not a ready-to-deploy tool.
