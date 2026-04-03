# Vocal Synth Looper

A mobile-first web app that turns your voice into a synth sequencer. Hold to record, sing or beatbox, and the app captures your pitch, rhythm, and timbre — then plays it back as a layered synth loop.

**Live:** [isaacparker.github.io/playbox](https://isaacparker.github.io/playbox/)

## What it does

- **Voice → Synth** — hold the center button, make a sound. The app detects pitch per step, maps rhythm to a 16-step sequencer, and shapes the synth to match your voice character (bright voice → sawtooth, soft hum → sine, etc.)
- **Per-step pitch capture** — sing a melody and each step gets its own note. Not one pitch for the whole loop — actual melodic sequences from your voice.
- **Intent selector** — choose Kick, Snare, Hat, Bass, Lead, or Pad before recording. The app optimizes detection and applies tuned synth presets for that sound type.
- **30 preset patches** — 5 per sound type (808, Acid 303, Reese, Hoover, Juno Pad, etc.). Hot-swap in the synth panel.
- **Scale lock** — auto-detects the scale from your recordings, shows it in the UI, snaps everything to key. Per-step note editing is filtered to scale notes only.
- **Per-layer synth controls** — waveform, ADSR envelope, filter (cutoff + resonance + envelope amount), drive/distortion. Collapsible panels.
- **Dual detuned oscillators + sub** — tonal layers have 3 oscillators for thickness, not a single thin waveform.
- **Room reverb capture** — clap in your room, the app captures the impulse response and applies it as a convolution reverb with adjustable wet mix.
- **Scene management** — multiple pattern banks (verse/chorus), duplicate and switch.
- **AI ghost steps** — Magenta.js suggests additional steps that complement your pattern (shown as dashed outlines, tap to accept).

## Tech

- Single HTML file, no build step
- Tone.js for synthesis and transport
- Magenta.js for AI pattern suggestion (optional, graceful fallback)
- Meyda for audio analysis
- Web Audio API for pitch detection (YIN autocorrelation) and room impulse capture
- GitHub Pages for hosting

## Architecture

All state is in-memory JS. The synth engine chains: `Oscillator(s) → WaveShaper (drive) → Filter (LP -24dB) → Destination + ConvolverNode (room)`. Per-step notes and durations stored as arrays on each layer. Scale detection scores pitch classes against 7 common scale templates.

Recording pipeline: `Mic → MediaRecorder → decode → per-step energy analysis → onset detection → per-onset pitch detection (YIN) → confidence filtering → scale normalization → step/note/duration mapping`.

## Running locally

Open `index.html` in a browser. Needs HTTPS or localhost for microphone access.
