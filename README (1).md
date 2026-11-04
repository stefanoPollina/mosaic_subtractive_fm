# Continuum Mosaic

A two-layer **subtractive / FM synthesizer that runs entirely in your browser**, in a single HTML file. No install, no build step, no libraries, and no network access once the file is open.

It ships with a 400+ preset browser, a bass-music engine (reese, growl, sub), a step sequencer with live automation, an arpeggiator, an XY morph pad, a built-in oscilloscope and spectrum analyser, and a preset storage system that keeps a backup of every save.

<!--
Add screenshots to a docs/ folder and uncomment:
![PLAY page](docs/play.png)
![BANK page](docs/bank.png)
-->

---

## Quick start

**Run it locally**

1. Download the HTML file.
2. Open it in a current Chrome, Edge or other Chromium-based browser.
3. Press **START** on the overlay (browsers only allow audio after a click or key press).
4. Play with the on-screen keys or your computer keyboard.

**Host it on GitHub Pages**

1. Put the file in the repo as `index.html`.
2. *Settings → Pages →* deploy from your main branch.
3. Open the Pages URL. Everything runs client-side.

The file is about 5 MB because the whole preset library is built in.

---

## Playing it

| Keys | Action |
| --- | --- |
| `A W S E D F T G Y H U J K` | Play notes |
| `Z` / `X` | Octave down / up |
| `Space` | Start / stop the sequencer |
| `Q` | Arpeggiator on / off |
| `Esc` | Panic (all notes off) |
| `Shift` + drag | Fine control on a dial |
| Double-click a dial | Reset it |

The on-screen keyboard works by mouse and touch. Web MIDI input is supported by the code, but hardware MIDI has not been tested.

---

## What's in it

### Sound engine

- **Two layers (A / B)**, each with **3 oscillators**, a filter, amp and filter envelopes and two LFOs.
- **Oscillator 1** has 4-voice unison with detune.
- **Waves:** saw, square, triangle, sine, **white noise** and **pink noise**.
- **Extra oscillator dials** (three per oscillator):

  | Oscillator | Extra dials |
  | --- | --- |
  | 1 | **FOLD** (wavefolder), **XMOD** (oscillator 2 frequency-modulates oscillator 1), **SUB** (sine one octave down) |
  | 2 and 3 | **FOLD**, **SYNC** (hard-sync to oscillator 1, 1×–8×), **RING** (ring-modulate with oscillator 1) |

- **Filter types:** LP, HP, BP, notch, comb, and **FMT** (a vowel filter with a **VOWEL** dial sweeping A → E → I → O → U).
- **Extra filter dials:** **DRIVE** (saturation *before* the filter) and **SLOPE** (blend from 12 dB to 24 dB per octave).
- **Engine modes:** phase distortion, FM, or both, with bend / pulse / sine shaping.
- **Effects:** drive, phaser / chorus, delay, multi-tap delay, reverb, shimmer, compressor and a master limiter.

All the new oscillator and filter dials default to zero, so presets that don't use them sound exactly as they did before.

### The BANK page

- **Preset browser** with search, categories, star ratings, favourites, "similar presets", and a **FACTORY / USER** switch (see [Storage](#storage-and-backups)).
- **Categories:** SIGNATURE, Bass, Lead, Pad, Pluck, Atmosphere, Sequence, **GENRE**, FX, Experimental, Factory. The **GENRE** category holds the Dub, Jungle, DnB, Psy and Trip Hop presets, each titled with its genre after its name (for example *Steppa Growler · DnB*).
- **Specification bank:** 148 presets with four named macros, an 8-route modulation matrix, XY morph corners, arpeggiator and sequencer settings, performance mapping and randomisation limits, all validated.
- **XY morph pad** that blends four captured snapshots (A–D). It reacts to sound: ripples follow your drag and note attacks, and a soft halo follows the real output level. It also follows automation and the arp.
- **Arpeggiator** (up, down, up-down, random, bank pattern; latch / hold) on a bar above the keyboard, available on every page.
- **Sequencer:** step sequencer with swing, gates, Euclidean generation and per-step probability. **Live automation lanes** can record and replay many parameters (macros, XY, arp, rhythm, LFO and expansion controls).
- **Randomise / mutate** within each preset's safe bounds.

### DnB engine

- **20 DnB presets** built on a reese stack (4-voice unison plus a detuned second saw), an FM growl layer and a mono sine sub on its own layer.
- Four macros: **Growl, Reese, Sub Weight, Screamer**. The Growl ends follow the spec ranges (300 Hz ↔ 2800 Hz, resonance 0.35 ↔ 0.98, drive 0.2 ↔ 1.0).
- A 32-step "steppa" pattern (swing 58 %) loads from **LOAD BANK SEQUENCE**.
- **DnB ENGINE switch** (next to the MACROS label): turns the same macros, XY map and steppa loader on for **any** preset. It starts neutral (centre = the preset's own sound), switches off cleanly, and is saved with user presets.

### Meters

- **Oscilloscope:** stereo (or summed), triggered to a fraction of a sample so held notes stand still, min/max drawing so peaks are never skipped, a gentle fading echo, gain, hold, and a taller mode.
- **Spectrum analyser:** log frequency axis 20 Hz–20 kHz calibrated in dBFS, FFT sizes 2048–16384, a 1/3-octave band mode, a fading peak-hold line, and a pitch read-out (frequency, note name, cents).
- Both roll up into a title bar with a live mini read-out, draw nothing while rolled up, and remember their state.

### Layout

Scales to your window (a 1280 × 820 layout, centred and fitted at any laptop or desktop size) and switches to a single-column layout on phones and narrow portrait windows.

---

## Storage and backups

Presets live in the browser's **IndexedDB** (database `HelixSynth`), with `localStorage` kept as a quick cache.

- **Factory bank is read-only.** Built-in presets show an **RO** badge. Loading and tweaking is fine, but they can't be overwritten; **SAVE** makes your own copy in the USER bank.
- **Your presets are versioned.** Every save keeps the version it replaces (default: the latest 100 per preset; "ALL" is a setting). Restore, audition, copy or export any version from **USER → BACKUPS**. Deleted presets stay recoverable.
- **Autosave** after a short idle (default 2 s): user presets are saved in place (the old version is backed up); factory presets keep a recovery copy instead. A **RECOVER UNSAVED** button appears after a crash or reload.
- **Undo / redo**, four **performance snapshots** (S1–S4), **tags** (searchable), **favourites**, and a **recent** list.
- **Import / export:** `.hlx` (a single preset, format `HelixPreset` 1.0), `.hlxbank` (the whole user bank with its version history), plus the original JSON preset files. You can also **drag and drop** files onto the page.
- Factory updates replace only the factory bank; your user bank is never touched.
- Cloud sync is only a flag (**SYNC READY**). Nothing is uploaded.

Everything is stored locally in your browser. Clearing site data erases it, so use **EXPORT ALL** to keep a backup file.

---

## Browser support

| Browser | Status |
| --- | --- |
| Chrome / Edge / other Chromium | Developed and tested here |
| Firefox, Safari (including iOS) | Not tested |

It needs Web Audio with AudioWorklet and IndexedDB. If IndexedDB is blocked (for example private browsing), it falls back to memory and says so in the storage panel.

---

## Testing

The features were verified with automated headless-Chromium tests (Playwright) that measure the real audio output, for example the spectrum of a sine for the analyser, harmonics for the wavefolder, and a side-by-side comparison with the original engine to confirm the new dials are neutral at zero. The test scripts are not part of this repository yet.

---

## Known limitations

- Tested in headless desktop Chromium only. Real phones, iOS Safari, Firefox and hardware MIDI have not been tried.
- The new oscillator and filter dials can't yet be automated or MIDI-mapped.
- The filter's 24 dB slope is a blend of a second 12 dB stage, and the DnB spec's separate pre/post distortion chain and direct sub output are approximated: the sub passes through the shared effects tail.
- On the PLAY page the global dial strip at the bottom is partly hidden behind the keyboard footer.
- No sound design has been judged by ear in the automated tests; treat hand-set preset values as starting points.

---

## Roadmap ideas

- Automation / MIDI mapping for the new oscillator and filter dials
- More variety across the DnB presets
- Real 24 dB filter and a formant bank in the worklet chain
- Move the test scripts into the repo

---

## Contributing

Issues and pull requests are welcome. Please describe the browser, window size and the steps that reproduce a problem.

## License

*Add a licence file before publishing (for example MIT) and reference it here.*
