# ✿ Magic Hands

Real-time hand-gesture effects and a gesture-dance scoring game that run entirely in the browser.
Built with [MediaPipe Tasks Vision](https://developers.google.com/mediapipe) and vibe-coded with Claude.

**Play:** https://ceshih1116.github.io/magic-hands/

## What's inside

| Page | What it does |
|---|---|
| `magic-hands.html` | Webcam effects: open palm grows flowers, ✌️ collects them, 🫰 finger heart pops sticker hearts, 🫶 two-hand heart bursts hearts, 💌 words on fingertips, 🌫 breathe on the "window" and write in the fog |
| `happy-birthday.html` | AR birthday in a refined, high-tech style (pastel light-leak grade, film grain, hand wireframe HUD). Open your palm and small line-art butterflies land on your fingertips; point one finger to blow bubbles (some hold photos) and poke them to pop; frame with both thumbs and index fingers (index fingers pointing opposite ways) and the space traced by your fingers opens onto a starry sky (galaxy, aurora, pink dusk); show two peace signs for a glitchy outline of your age (set on the start screen) and a three-tier pastel cake with gold candles; close your eyes for your wish written across a constellation; thumbs up for floating line-art hearts, an OK sign for a halo ring, a clap for a burst of glitter; tap your wrist for a holographic watch at 22:22; blow (rounded lips held 0.6 s) to dissolve the candle light. Photos are picked by the user and never leave the browser |
| `dreamscape.html` | AR daydream in your own room, after the 徐綠楊柳柳鵝 style videos. Point one finger or tap the screen for a water drop that really bends the picture (WebGL refraction) with splash droplets; raise an open palm for a soft cloud sky and a flock of doves that circles your hand; hold your hands flat for a dark-blue wireframe mountain world whose peaks rise under your hands; raise two fists for a blue glow with light rays; lift both hands beside your head to turn into a white silhouette that breaks into light and vanishes (the room behind you is filled in); close your eyes for a soft-focus dream with bokeh; music sync pulses everything with the beat |
| `gesture-dance.html` | Load a gesture-dance tutorial video, dance along in front of the camera, and get Perfect / Good / Miss on every beat, a final grade, and an optional recording |

## How it works

- **Hand tracking:** MediaPipe Hand Landmarker (21 landmarks per hand, 2D + 3D world coordinates).
- **Mouth detection (fog mode):** MediaPipe Face Landmarker blendshapes (`jawOpen`, `mouthFunnel`), with smiling suppressed and a 0.25 s hold to avoid false triggers.
- **Gesture rules:** finger-curl ratios computed from 3D world landmarks so they hold up when the hand rotates. The finger-heart vs. fist vs. "L" shape rules were calibrated frame-by-frame on real screen recordings:
  - finger heart detection went from 34% to 79–83% of frames
  - false positives on the "L" shape dropped from 80% to 2%
- **Dance scoring:** Pose Landmarker runs on both the tutorial video and the webcam. Poses are normalised by shoulder width and compared by arm-segment angles plus hand position relative to the shoulders. The best match within a 0.7 s window is used, so following half a beat late isn't penalised. Thresholds were calibrated with simulated followers:

  | Simulated follower | Grade |
  |---|---|
  | perfect | S |
  | real human follower | A |
  | standing still | D |
  | unrelated motion | D |

- **Dreamscape:** Hand Landmarker (GPU) with 3D finger-curl ratios for point / open / fist and the palm direction (fingers up vs. flat); Image Segmenter with the selfie segmenter (landscape) for clean cut-outs, falling back to the Pose Landmarker mask; a background plate built from the mask (seen pixels plus a low-resolution diffusion fill) for vanishing; Face Landmarker blendshapes for the dream and the face box for the vanish pose; a WebGL pass that refracts the composed frame for the water drops; fbm-noise clouds rendered once; Web Audio bass-energy onsets for beat sync.

## Privacy

All video processing happens locally in the browser. Nothing is uploaded; only the MediaPipe model files are downloaded from Google's CDN.

## Run locally

Open either HTML file in desktop Chrome and allow camera access. Phones need the HTTPS link above, because mobile browsers block the camera on local files.

## Credits

Tutorial videos used with `gesture-dance.html` are loaded by the user and remain the property of their creators.
