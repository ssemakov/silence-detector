# Silence Detector Implementation Plan

This document captures the three-phase roadmap for expanding the silence detection utility that leverages `ffmpeg` for audio analysis.

## Phase 1 – Robust Silence Detection Foundation
- Replace the placeholder CLI with proper argument parsing (MP4 path, silence thresholds, output reporting options) and wire those values into the detector logic.
- Expand the detector package to execute `ffmpeg` (e.g., `ffmpeg -i <video> -af silencedetect=... -f null -`) and parse its `silencedetect` logs into structured silence intervals, enriching the detector beyond threshold-only checks.
- Add unit and integration tests that mock or fixture `ffmpeg` output so the detector logic can be validated without invoking the binary, extending the current threshold-oriented tests to cover parsing and interval reporting.

## Phase 2 – GitHub Action Automation
- Create a workflow that checks out the repository, installs Go and `ffmpeg` on Ubuntu runners, builds the CLI, and runs unit tests to keep silence detection reliable.
- Add a job or `workflow_dispatch` input allowing maintainers to supply CLI parameters so the action can run the detector against a provided sample video or artifacts.
- Publish run artifacts (logs, JSON summaries) for troubleshooting whenever the action is triggered manually or on pull requests.

## Phase 3 – Optional Pattern Matching Enhancement
- Extend the CLI interface so users can supply an MP3 pattern via local path or URL, enforcing a five-second duration cap during ingestion and converting it to a normalized PCM buffer for analysis.
- Implement drift-tolerant matching (e.g., cross-correlation with sliding windows or dynamic time-warping) between the normalized pattern and the video’s audio track extracted through `ffmpeg`, producing timestamps where the pattern is detected.
- Surface the combined silence intervals and pattern hits in CLI output and structured reports, and add tests covering URL/file loading, duration validation, and drift-aligned detection logic.

## Testing
- Not run; documentation-only update.
