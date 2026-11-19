# ✿ Magic Hands

Real-time hand-gesture effects and a gesture-dance scoring game that run entirely in the browser.
Built with [MediaPipe Tasks Vision](https://developers.google.com/mediapipe) and vibe-coded with Claude.

**Play:** https://ceshih1116.github.io/magic-hands/

## What's inside

| Page | What it does |
|---|---|
| `magic-hands.html` | Webcam effects: open palm grows flowers, ✌️ collects them, 🫰 finger heart pops sticker hearts, 🫶 two-hand heart bursts hearts, 💌 words on fingertips, 🌫 breathe on the "window" and write in the fog |
| `twenty-two.html` | AR birthday in a refined, high-tech style (pastel light-leak grade, film grain, hand wireframe HUD). Open your palm and small line-art butterflies land on your fingertips; point one finger to blow bubbles (some hold photos) and poke them to pop; frame with both thumbs and index fingers (index fingers pointing opposite ways) and the space traced by your fingers opens onto a starry sky (galaxy, aurora, pink dusk); show two peace signs for a glitchy outline of your age (set on the start screen) and a three-tier pastel cake with gold candles; close your eyes for your wish written across a constellation; thumbs up for floating line-art hearts, an OK sign for a halo ring, a clap for a burst of glitter; blow (rounded lips held 0.6 s) to dissolve the candle light. Photos are picked by the user and never leave the browser |
| `dreamscape.html` | AR daydream: your room is replaced with sky and layered mountains (pose segmentation). Wave to steer a flock of birds, turn your wrist to turn them, open your palm to grow a mountain ridge from your palm lines and spread your fingers to add far-away layers, close your eyes to grow misty blue wings, and turn on music sync so everything pulses with the beat (microphone onset detection) |
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

- **Dreamscape:** Hand Landmarker (GPU) for waves, wrist roll and finger spread; Pose Landmarker with segmentation masks (CPU) for background replacement and shoulder anchors; Face Landmarker blendshapes (`eyeBlinkLeft/Right`) for the wings; Web Audio bass-energy onsets for beat sync. Finger-spread and eye-closure thresholds were calibrated on a real recording.

## Privacy

All video processing happens locally in the browser. Nothing is uploaded; only the MediaPipe model files are downloaded from Google's CDN.

## Run locally

Open either HTML file in desktop Chrome and allow camera access. Phones need the HTTPS link above, because mobile browsers block the camera on local files.

## Credits

Tutorial videos used with `gesture-dance.html` are loaded by the user and remain the property of their creators.
