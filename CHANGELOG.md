# Changelog

All notable changes to StruckSynth. Versions follow the git tags; the format is
[Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

### Changed

- Help opens the help page in a window of the plugin's own, not in the web browser. Nothing is
  written to the temp folder, and on Linux no web server runs in the host any more.

## [0.9.0] - 2026-11-14

First public preview.

### Added

- Modal-synthesis instrument: each note fires a shaped strike into up to 64 tuned resonant modes.
- Mallet or noise strikes and material presets for bells, bars, tines and blocks.
- MIDI 2.0 notes taken directly, so a Pitch 7.9 note plays its exact pitch; MPE and CLAP note
  expressions too.
- CLAP plugin for Linux (x86-64) and macOS 26 or later (Apple Silicon).
