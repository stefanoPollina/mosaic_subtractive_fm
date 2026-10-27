# Continuum Mosaic
A fixed-panel synthesizer in one HTML file. Open `index.html` and the instrument is there: oscillators, filter, envelopes, modulation, effects, and a step sequencer. No build step, no modules, no server.
It is a playable reduction of the larger Continuum Mosaic environment. The modular rack, the patch cables, the percussion desks, and the 48-channel mixer are not in this page. What remains is the part you can perform: two layered voices, a small modulation matrix in place of the patch bay, and the effects that were already doing the musical work.
## Play it
1. Open `index.html` in a current Chrome, Edge, Firefox, or Safari. Double-clicking the file is enough. GitHub Pages is enough.
2. Click **START**. Browsers will not start audio until you gesture. START also plays a short note, so you can hear that the output path is alive.
3. Play the on-screen keyboard, or use the computer keys.
| Keys | Action |
| --- | --- |
| A W S E D F T G Y H U J K O L P ; ' | C to F, one octave and a fifth |
| Z / X | Octave down / up |
| Space | Start or stop the sequencer |
| Escape | Silence every voice |
A MIDI keyboard works through Web MIDI: notes, pitch bend, the mod wheel, and the sustain pedal. The dot beside the meters lights when an input is connected.
Drag a knob up and down. Scroll changes it in smaller steps. Double-click returns the default. Shift makes the movement finer.
## The panel
The face is a fixed 1280×820 instrument and scales to fit the window. Four pages:
- **PLAY** edits one layer at a time. A and B are the two engines. Each has three oscillators, a multimode filter, an amp envelope, and a filter envelope. The voice strip under them sets polyphony, glide, unison, and detune for the whole instrument.
- **MOD** is eight modulation slots. Sources are the two LFOs, both envelopes, velocity, the mod wheel, and key tracking. Destinations include pitch, cutoff, resonance, pulse width, FM index, pan, and oscillator level. A slot can aim at layer A, layer B, or both. The filter’s own ENV and LFO amounts stay on the filter, because those were the connections you always wanted without patching them.
- **FX** is the output chain: drive, phaser, synced delay, a three-band delay, reverb, a pitched shimmer, and a compressor, then a limiter.
- **SEQ** is sixteen steps with accent, slide, swing, and a rhythm gate. Length can be 8 or 16. Acid Line and Spark Arp are sequences: open SEQ and press play.
The scope and the voice/CPU meters sit in the header. CPU is a rough share of the audio block, not a DAW-style measurement.
## Factory bank
Twenty presets ship inside the file. Init is the blank patch. The rest are starting points, not a library to browse for its own sake:
Continuum Pad, Acid Line, Supersaw Lead, Glass FM, PWM Keys, Dual Strings, Comb Drift, Soft Bell, Night Air, Sub Bass, Reese, Hoover, Chapel Organ, Mute Pluck, Fifth Bell, Slow Choir, Brass Stab, Pulse Sweep, Spark Arp.
SAVE stores a preset in this browser only. EXPORT writes the current patch as JSON. IMPORT reads one patch or a list of them. Nothing is uploaded.
## How the sound is made
The voice is one polyphonic processor: up to twenty voices, three oscillators each, a filter that can be lowpass, highpass, bandpass, notch, or comb, plus FM and phase distortion on layer oscillators. Unison spreads oscillator 1. Drift and a small amount of “character” roughen the pitch the way an analogue engine does, without modelling a particular circuit.
That processor was written as an AudioWorklet. Worklets need a separate script URL, and several browsers refuse that when the page is opened from disk. This file does not depend on that. On startup it runs the same processor on the main thread and schedules short stereo buffers into the effect chain. You get the same instrument from a folder, from GitHub Pages, and from a desktop browser that blocks worklet modules.
The cost is honest: the voice shares the page’s thread with the interface. Dense unison patches at high polyphony will show up on the CPU meter before a native plugin would. The buffer scheduler keeps a short horizon of audio queued so a single late frame does not click.
Effects are ordinary Web Audio nodes after the voice. Reverb is a generated impulse, not a sampled room. Shimmer is a quiet pitched layer, not a convolution of the input. Drive is a waveshaper.
## What this page is for
Use it when you want the instrument without the workspace around it. Drop `index.html` in an empty repository and turn on GitHub Pages. The site is the file.
Edits are edits to that file. Presets, the panel, and the voice live in the same document so the page cannot get out of step with a missing script. The last patch you played is remembered in `localStorage` on that browser. Clearing site data clears it.
## Limits
- Audio starts only after START, or after another gesture the browser accepts.
- Opening the file from disk works. Some browsers still throttle timers in background tabs, and the sequencer will drift if the tab is hidden.
- Web MIDI needs a browser that implements it, and a gesture before access is granted.
- There is no sample import, no MPE, and no way to route this page into the original modular rack.
