# Scripts

There is no rendering code here yet. How frames are rendered is an open decision.

## Option A: bundle a cut-down painterly renderer

Put a reduced version of a painterly renderer in this folder.

- Keeps the painted look that the house style is tuned for.
- More work to cut down, document and keep working.
- Must not copy code from any private repo without Josh's say-so.

## Option B: an engine-free pipeline

Render frames with HTML canvas or Python, then join them into a video with ffmpeg.

- Fewer dependencies, and easier for anyone installing the skill to run.
- The painterly look becomes optional, applied as separate finishing passes where possible.

Until this is decided, leave this folder without code.
