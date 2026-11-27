# Cymatics prototype (parked, trial 30)

A watch-only cymatic view on each of the three MACRO XY pads. It's parked so the synth stays easy to use. It may become its own synth project later.

`mosaic-macro-xy-trial-30.html` is the full synth with the cymatics built in. Open it locally. It is not the live site.

## What works
- An overhead ring light reflecting off a vibrating water surface. The shapes come from the vibration patterns of a round dish (Bessel modes), with a central mode and fewer rings. This is the approved "v4" look.
- One look per pad:
  - SOUND: amber lace with white highlights.
  - ACCENT (a fifth above): pearl-lavender lace with an amber rim.
  - FX (an octave above): cyan lace with an amber and white centre.
- A strobe-style light, so the pattern breathes but never goes flat.
- In silence each pad holds its last shape, dimmed. It moves only while there is sound.
- A teal toggle with a ring (◎) on each pad. The pad is locked while the view is on.

## Why it's parked
- **Audio dropouts:** in testing, the three pads raised dropouts from about 1 to 6 per 9 seconds. They also caused stalls of 54–78 ms, moments where the page freezes and can delay the sound. They are too heavy to run next to the synth on the main thread.
- **Same shape on every key:** the shapes were driven by guessing the note from the sound, and that guess came back as 0 while playing.

## If it's revived
- Drive the shapes from the note actually played. Add the note number to the `mosaic-xy-key-attack` event; `xyAttack` is called from the keyboard, the ARP and the sequencer.
- Render in a Web Worker with OffscreenCanvas, so it never blocks the audio.
- Decide whether it is watch-only or an interactive instrument. If interactive, it's probably a separate synth.
