# Painterly production

How to render an explainer with the painted look, using the kit in [josh-janse/painterly-explainers](https://github.com/josh-janse/painterly-explainers). Episode 4, "The Uncle Who Only Reads Letters", is the working example: `ep4/` in that repo.

## Set up

```sh
git clone https://github.com/josh-janse/painterly-explainers && cd painterly-explainers
pip install numpy scipy pillow fonttools   # plus ffmpeg
(cd engine && ./fetch_textures.sh)
python3 tools/fetch_sfx.py                 # run on a local machine: Freesound blocks cloud IPs
```

The kit carries its own copy of the painterly engine (`engine/`), the fonts and the sound list. Commit work to a repo as you go: a cloud workspace can be wiped between sessions, and once ep4 had to be rebuilt from notes.

## How an episode is built

One package per episode (`ep4/`), copied and renamed for the next one:

| Module | Job |
| --- | --- |
| `timeline.py` | Scene start and end times, taken from the pauses in the recorded voice-over |
| `layout.py` | Every position, size and path, for landscape and reel |
| `backdrops.py` | Paints the plates once with the engine (sky, sea, figures, paper), plus masks |
| one module per scene group | Returns each frame as an array for a given time |
| `transitions.py`, `sheets.py` | Camera-blur rushes, paper-sheet wipes, dips |
| `video.py` | Picks the scene for each frame and pipes frames to ffmpeg |
| `audio.py` | Voice chain, effects, ambience and music, mixed to one WAV |

Here the voice-over was recorded before the picture was made, so `timeline.py` reads its scene times from the pauses in the recording. If you record the voice-over last, as [sound-and-voice.md](sound-and-voice.md) describes, set the times by hand and re-time after recording.

Commands:

```sh
scripts/build_backdrops.sh [--format reel]           # paint the plates, about 4 min
STEMS=1 python3 -m ep4.audio out/ep4/mix.wav
scripts/render_ep4.sh                                # landscape 1920x1080
EP4_FORMAT=reel scripts/render_ep4.sh                # reel 1080x1920
python3 -m ep4.video out/t.png 21.0                  # one frame, for checking
```

## Formats

- Landscape is 1920×1080 at 24 fps, with overlays stepped on twos (12 fps).
- The reel is 1080×1920. Keep text and key action inside the safe box (x 60–960, y 250–1470). Opening and key frames must also read in the centre 3:4 crop (y 240–1680).
- Recompose the reel from blockouts rather than cropping, and repaint its plates at portrait size.
- Text on a phone is 40 px or larger.

## Checks that caught real problems

- Compare frames against the approved version side by side at fixed times. After any change, diff unchanged scenes pixel for pixel.
- `paint()` cuts its mask off at 0.5. To fade a shape (the hill's long tail into the haze), pass the fade as `alpha`, not inside the mask.
- Parallax uncovers the sky behind the near plane. Fill it by inpainting; copying columns leaves vertical streaks.
- Peak control goes through a look-ahead limiter at −1 dBTP, never a tanh soft-clipper, which made the voice crunchy.
- Tape sits on top of its card, straddling the top edge, and never covers words.
