# honeywav
A Python audio processing library, Core in c, and generate wav file.

Example using this library:
```python
import honeywav
import honeywav.dsp as dsp
import math

SR = 48000

# -- -- -- -- -- -- -- -- -- - AI Tone Example -- -- -- -- -- -- -- -- -- -
with honeywav.open("ai_tone.wav", sample_rate=SR) as w:
    phase = 0.0
    step = 2 * math.pi * 440 / SR

    for _ in range(SR):
        w.write_frames([0.5 * math.sin(phase)])
        phase += step

# -- -- -- -- -- -- -- -- -- - Text-to-Speech Example -- -- -- -- -- -- -- -- -- -

with honeywav.open("hello_world.wav", sample_rate=SR) as w:
    dsp.silence(w, 0.1)
    dsp.tone(w, 180, 0.10, 0.4)  # h
    dsp.tone(w, 300, 0.12, 0.4)  # e
    dsp.tone(w, 220, 0.10, 0.4)  # l
    dsp.tone(w, 220, 0.10, 0.4)  # l
    dsp.tone(w, 160, 0.15, 0.4)  # o

    dsp.silence(w, 0.15)
    dsp.tone(w, 140, 0.12, 0.4)  # w
    dsp.tone(w, 160, 0.15, 0.4)  # o
    dsp.tone(w, 200, 0.10, 0.4)  # r
    dsp.tone(w, 220, 0.10, 0.4)  # l
    dsp.tone(w, 120, 0.15, 0.4)  # d
```
