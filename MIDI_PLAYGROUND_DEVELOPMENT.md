# MIDI Playground Development Notes

## Scope
Added a MIDI playground to `FactorizationIn2D.html` without changing the existing plot engine behavior.

New capabilities:
- Map **unique plot heights** to a full **88-key piano** range (`A0..C8`, MIDI `21..108`)
- Treat horizontal axis as time (`n = 1..N`) and vertical axis as piano pitch
- Control tempo with **BPM** and rhythmic subdivision (`1/4`, `1/8`, `1/16`)
- Choose curve source from **real / imaginary / modulus / phase** for both `s_n` and `\dot{s}_n`
- Apply strict scale-note gating (notes in scale are ON, all others OFF) with selectable root
- Choose mapping range: Piano 88 (`A0..C8`) or optional Full 8 Octaves (`C0..B7`)
- Optional layout mode: `Consecutive Scale Keys` (virtual key ladder with scale notes only)
- Prime-aware accent clap so prime indices sound distinct
- Prime percussion selector (Clap/Snare/Kick/Hats/Tom/Clave/Cowbell)
- Instrument selector for playback/MIDI program
- Extended instrument bank (synth + sample presets)
- Professional-style FX chain (reverb, delay, chorus, drive, compressor)
- Play sequence with local synth (Web Audio)
- Optional external MIDI output (Web MIDI)
- Export sequence as a standard `.mid` file
- Auto-export sidecar session `.json` with full generation/config state
- One-click bundle export as `.zip` containing `.mid + .mp3 + .json` (MP3 included when encoder is available)
- Export mapped note-octave sequence as `.csv` and `.md`
- Import `.mid` files and replay them in the playground
- Import session `.json` and rebuild the exact generation settings
- Play up to 4 curves simultaneously (Curve 1 + optional Curves 2-4)
- Per-curve settings for source/root/scale/instrument/velocity/gate/prime-percussion

---

## Files Changed
- `FactorizationIn2D.html`
- `MIDI_PLAYGROUND_DEVELOPMENT.md` (this document)

---

## UX Added
Inside the left control sidebar:
- `🎹 MIDI Playground` button to open a dedicated modal.

Inside MIDI modal:
- Source selector:
  - `Re(s_n)`, `Im(s_n)`, `|s_n|`, `Phase(s_n)`
  - `Re(\dot{s}_n)`, `Im(\dot{s}_n)`, `|\dot{s}_n|`, `Phase(\dot{s}_n)`
- Instrument selector (`Acoustic Piano`, `Electric Piano`, `Organ`, `Strings`, `Synth Lead`, `Bass`, `Flute`, `Pluck`)
- Extended instruments: `Warm Pad`, `Glass Pad`, `Analog Brass`, `Soft Bell`, `Dark Bass`, `Air Lead`
- Sample presets: `Sampled Piano (GM)`, `Sampled E-Piano (GM)`, `Sampled Synth Pad (GM)`, `Sampled Synth Lead (GM)`
- FX Preset selector (`Studio`, `Ambient`, `Wide`, `Punch`, `Dry`)
- FX controls: Reverb %, Delay %, Chorus %, Drive %
- Precision (controls uniqueness binning for floating heights)
- BPM
- Subdivision
- Velocity
- Gate %
- MIDI output device selector
- Build Map
- Play / Stop
- Export `.mid`
- Export `.json`
- Export Bundle `.zip`
- Notes CSV export
- Notes MD export
- Load MIDI (file input)
- Load Session JSON (file input)
- Extra Curves panel for Curves 2-4 (enable/disable each curve)
- Piano-roll canvas (time × 88-key vertical pitch)
- Live scale legend (12 pitch classes with ON/OFF state and root highlight)

---

## Mapping Model

### 1) Height extraction
For each `n` in `1..N`, extract one scalar height based on selected source:
- `sn_re` → `Re(s_n)`
- `sn_im` → `Im(s_n)`
- `sn_mag` → `|s_n|`
- `sn_phase` → `arg(s_n)`
- `sdot_re` → `Re(\dot{s}_n)`
- `sdot_im` → `Im(\dot{s}_n)`
- `sdot_mag` → `|\dot{s}_n|`
- `sdot_phase` → `arg(\dot{s}_n)`

### 2) Unique height discovery
- Heights are rounded to selected precision (`toFixed(precision)`)
- Rounded set is made unique
- `0` is always included (so height zero always has a mapping)

### 3) 88-key assignment
Sort unique heights ascending and map rank to the piano index:

\[
\text{pianoIndex} = \operatorname{round}\left(\frac{\text{rank}\cdot 87}{\max(1, U-1)}\right),\qquad
\text{midi} = 21 + \text{pianoIndex}
\]

where `U` is number of unique heights.

### 3b) Scale quantization (optional)
Mapping now uses **only enabled notes** in the selected scale/root and range.

- Build list of available MIDI notes where pitch class belongs to the selected scale intervals.
- Heights are rank-mapped directly onto this available-note list.
- This is an ON/OFF gate behavior for notes (not nearest snapping).

- Root: `C..B`
- Scale: English list from your reference image (plus chromatic)

Included scales:
- Chromatic (All 12)
- Arabic
- Harmonic Minor
- Augmented
- Bebop Dominant
- Bebop Major
- Blues
- Diminished
- Dorian (Jazz)
- Enigmatic
- Major
- Minor
- Phrygian
- Japanese
- Lydian
- Locrian (Japanese/Hindu)
- Hungarian Minor
- Melodic Minor
- Mixolydian (Jazz/Blues)
- Neapolitan
- Neapolitan Minor
- Pentatonic (Blues/Rock)
- Pentatonic Major
- Pentatonic Minor
- Whole Tone

### 3c) Optional mapping ranges
- `Piano 88`: `A0..C8` (MIDI `21..108`)
- `Full 8 Octaves`: `C0..B7` (MIDI `12..107`)

### 3d) Consecutive Scale Keys mode
- In `Physical Keys`, mapping uses the real MIDI-key distribution inside selected range.
- In `Consecutive Scale Keys`, the key ladder is compressed to scale notes only (no chromatic gaps).
- The engine stacks allowed scale notes across as many octaves as needed for discovered unique heights.
- `Base Octave` sets where the virtual ladder starts (`C0..C7`), then climbs through scale notes consecutively.
- If unique-height count exceeds available MIDI note space, mapping **rolls over** from the start of the allowed-note ladder and continues cyclically.
- Piano roll shows rollover boundaries as vertical markers at cycle transitions.

### 4) Note classes and requested indexing
For each mapped note:
- `index_0 = midi % 12` gives `0..11`
- Clockwise/chromatic names: `C, C#, D, D#, E, F, F#, G, G#, A, A#, B`
- Scale labels used: `1, b2, 2, b3, 3, 4, b5, 5#, 6, 6#, 7, 7#`
- `index_13 = index_0 + 13`, giving `13..24`

---

## Timing Model
Let `subdivision` be:
- `1` for quarter notes (`1/4`)
- `2` for eighth notes (`1/8`)
- `4` for sixteenth notes (`1/16`)

Then each step duration is:

\[
\Delta t = \frac{60}{\text{BPM}\cdot \text{subdivision}}
\]

Each sequence element (`n`) advances one step in time.
Gate controls note length within each step:

\[
\text{gateSeconds} = \Delta t\cdot \frac{\text{gatePercent}}{100}
\]

In multi-curve mode, all enabled curves share the same global BPM/subdivision clock,
and each curve applies its own velocity/gate/prime settings at each time step.

---

## Playback

### Local synth (always available)
- Uses Web Audio API
- Instrument-dependent oscillator/envelope
- Velocity mapped to amplitude

### Sampled instruments
- Optional GM soundfont sample playback (online source)
- Per-note sample fetch/decode with cache
- Automatic fallback to synth playback if sample is unavailable

### Effects chain
- Dry path + Convolution Reverb + Tempo Delay + Stereo Chorus + Waveshaper Drive
- Dynamics compression on output for level control
- Live-adjustable wet amounts via sliders

### Prime accent mode
When **Prime Accent** is enabled:
- Prime-index events (`n` prime) trigger a short percussion layer
- Prime notes still receive slight velocity emphasis
- In piano roll, prime events are highlighted in orange

Prime accent now supports selectable percussion types:
- `Clap`, `Snare`, `Kick`, `Closed Hat`, `Open Hat`, `Tom`, `Clave`, `Cowbell`
- Local audio uses synthesized percussion profiles per type
- MIDI output/export uses corresponding drum-channel note mapping

### External MIDI out (optional)
- Uses Web MIDI API if browser/device allows it
- Sends Note On / Note Off per event
- Device selected in modal output dropdown

---

## MIDI Export
- Exports Standard MIDI File (SMF), format 0, one track
- Tempo meta-event from BPM
- Ticks per quarter note = `480`
- Step ticks = `480 / subdivision`
- Gate ticks = `stepTicks * gatePercent`
- Program change follows selected instrument preset
- Selected prime percussion is serialized as matching drum-channel note (channel 10)
- Export timing now uses absolute tick scheduling converted to valid delta times
- Every `.mid` export also downloads a same-name `.json` sidecar payload containing:
  - Generator params (`N`, `To`)
  - Full MIDI UI config (source/root/scale/range/layout/base octave/timing/velocity/gate/prime/instrument/FX)
  - Derived mapping data (`uniqueHeights`, allowed notes, `heightToMidi`)
  - Event list used for generation
- Additional mapped-sequence exports:
  - `{base}_notes_octaves.csv`
  - `{base}_notes_octaves.md`
- Multi-curve export writes simultaneous notes per step across channels:
  - Curve 1 → channel 1
  - Curve 2 → channel 2
  - Curve 3 → channel 3
  - Curve 4 → channel 4
  - Prime percussion accents stay on channel 10

## MP3 Export Reliability
- MP3 export now tries loading `lamejs` in this order:
  1) local `vendor/lame.min.js`
  2) jsDelivr CDN
  3) unpkg CDN
  4) cdnjs CDN
- Bundle export attempts to include MP3 automatically; if encoder loading/rendering fails, bundle still exports other files and reports MP3 omission.

Output filename pattern:
- `factorization_midi_N{N}_To{To}_{source}.mid`
- `factorization_midi_N{N}_To{To}_{source}.json`

---

## MIDI Import
- Parser supports Standard MIDI files with PPQ timing (non-SMPTE)
- Reads format 0/1 tracks, note on/off events, running status, and tempo meta events
- Converts imported notes to timeline seconds with tempo-map handling
- `Play Imported` schedules and plays uploaded MIDI through local engine + optional MIDI out

## Session JSON Import
- Session JSON loader restores generator params (`N`, `To`) and MIDI controls
- Rebuilds mapping immediately so playback/export are reproducible

---

## Integration and Safety
- Existing rendering/data logic remains intact
- MIDI logic is additive and isolated in a dedicated section
- Recompute/animation marks MIDI map dirty
- If MIDI modal is open and not playing, map rebuilds after recompute
- `Escape` closes both Explanation and MIDI modals

---

## Notes / Current Constraints
- Sequence is monophonic by design (one note per `n`)
- Pitch mapping is rank-based across discovered unique heights
- Very large `N` can generate dense rolls and long playback/export (expected)
- Scale quantization may collapse multiple unique heights into the same note class by design

---

## Quick Usage
1. Open `FactorizationIn2D.html`
2. Set `N` and `To`
3. Click `🎹 MIDI Playground`
4. Choose source + BPM + subdivision
5. Click `Build Map`
6. Click `Play` (or `Export .mid`)

---

## Implementation Summary
Added in `FactorizationIn2D.html`:
- MIDI modal + controls + piano roll canvas
- Height-to-key mapping engine
- Summary/status reporting
- Piano-roll renderer
- Web Audio playback
- Web MIDI output detection and routing
- MIDI file writer (`.mid`) and downloader
- State synchronization hooks with existing recompute/animation flow
