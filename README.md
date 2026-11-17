# honeywav
A Python audio processing library, Core in c, and generate wav file.

Example using this library from python:
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

The core c eaample:
```c

#include "dsp.h"
#include "wave.h"
#include <math.h>
#include <stdio.h>

#define SR 48000

// ------------------- Example 1: AI-tone generation -------------------
void example_ai_tone() {
  wave_writer_t w;
  wave_open(&w, "ai_tone.wav", SR, 1, WAVE_FLOAT32);

  int samples = SR; // 1 second
  float phase = 0.0f;
  float step = 2.0f * M_PI * 440.0f / SR; // 440Hz AI tone

  for (int i = 0; i < samples; i++) {
    float s = 0.5f * sinf(phase);
    phase += step;
    wave_write_frames(&w, &s, 1);
  }

  wave_close(&w);
}

// ------------------- Example 2: Audio recording simulation -------------------
void example_audio_recording() {
  wave_writer_t w;
  wave_open(&w, "audio_recording.wav", SR, 1, WAVE_FLOAT32);

  int samples = SR; // simulate 1 second of audio
  int i = 0;
  float phase = 0.0f;
  float step = 2.0f * M_PI * 660.0f / SR; // simulated live audio

  while (i < samples) {
    float s = 0.4f * sinf(phase);
    phase += step;
    wave_write_frames(&w, &s, 1); // write "live" buffer
    i++;
  }

  wave_close(&w);
}

// ------------------- Example 3: Text-to-Speech robotic demo
// -------------------
void example_text_to_speech() {
  wave_writer_t w;
  wave_open(&w, "hello_world.wav", SR, 1, WAVE_FLOAT32);

  dsp_silence(&w, 0.1f);

  dsp_tone(&w, 180, 0.10f, 0.4f); // h
  dsp_tone(&w, 300, 0.12f, 0.4f); // e
  dsp_tone(&w, 220, 0.10f, 0.4f); // l
  dsp_tone(&w, 220, 0.10f, 0.4f); // l
  dsp_tone(&w, 160, 0.15f, 0.4f); // o

  dsp_silence(&w, 0.15f);

  dsp_tone(&w, 140, 0.12f, 0.4f); // w
  dsp_tone(&w, 160, 0.15f, 0.4f); // o
  dsp_tone(&w, 200, 0.10f, 0.4f); // r
  dsp_tone(&w, 220, 0.10f, 0.4f); // l
  dsp_tone(&w, 120, 0.15f, 0.4f); // d

  wave_close(&w);
}

// ------------------- MAIN -------------------
int main() {
  printf("Generating AI-tone example...\n");
  example_ai_tone();

  printf("Generating Audio recording simulation example...\n");
  example_audio_recording();

  printf("Generating Text-to-Speech example...\n");
  example_text_to_speech();

  printf("All examples done!\n");
  return 0;
}
```

Copyright ©️ Tcoder64 2026 all right reserved.
