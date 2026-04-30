# Real-time MIDI Score Follower & Orchestra Accompaniment

## Overview

A real-time MIDI score follower and orchestra accompaniment system for solo pianists. It uses Hidden Markov Models (HMM) and Online Time Warping (OLTW) to track a live piano performance against a digital score and synchronize an automated orchestral accompaniment in real-time.

## Architecture

This is a **Python desktop application** using Pygame for its GUI. There is no web frontend or backend server.

### Core Components

- **`interactive_tester.py`** — Main GUI launcher (Pygame-based), the primary entry point
- **`hybrid_fusion.py`** — `HybridScoreFollower` combining HSMM + OLTW tracking approaches
- **`hmm_follower.py`** / **`hsmm_follower.py`** / **`oltw_follower.py`** — Individual tracker implementations
- **`midi/real_orchestra_player.py`** — Orchestral playback with sample loading and tempo adjustment
- **`midi_to_score.py`** — Converts MIDI files to JSON score format used by trackers
- **`midi_workspace.py`** — Manages project layout (piano + orchestra MIDI pairs)
- **`output_dispatcher.py`** — Routes audio to local samples or external DAWs (Logic Pro)
- **`main.py`** — CLI entry point for HMM-based score follower

### Algorithm

Hybrid Fusion approach:
1. **HSMM** (Hidden Semi-Markov Model) for probabilistic state tracking
2. **OLTW** (Online Time Warping) for robust recovery from large jumps or mistakes

### Audio Engines

- **Local samples**: Uses `pygame-ce` to trigger bundled audio samples
  - Piano: `assets/piano_samples/salamander_mp3/`
  - Orchestra: `assets/orchestra_samples/philharmonia_strings/`
- **External DAW**: Dispatches MIDI events to Logic Pro or other DAWs via IAC

## Project Layout

```
/                   - Core algorithms and entry points
assets/             - Audio samples (piano + orchestra)
midi/               - MIDI utilities and piece library
generated_dataset/  - Example JSON scores and MIDI test files
midi_library/       - User workspace storage (after import)
```

## Dependencies

- **Python 3.12**
- `numpy` — Numerical computations and matrix operations
- `mido` — MIDI file and stream handling
- `pygame-ce` — GUI and low-latency audio/MIDI output

## Running

```bash
python3 interactive_tester.py --launcher
```

## Workflow

The app runs as a VNC desktop application (Pygame GUI). The Replit workflow is configured as `vnc` output type.

## User Flow

1. On first launch, a setup wizard prompts for Orchestra Engine and Piano Input
2. Select `Local samples` for Orchestra Engine
3. Select your physical MIDI keyboard for Piano Input (avoid IAC/virtual inputs)
4. Open `Orchestra / Full Score` → `Load MIDI Pair` to import a piece
5. Select piano MIDI file, then orchestra MIDI file
6. Wait for import to complete, then perform with live accompaniment
