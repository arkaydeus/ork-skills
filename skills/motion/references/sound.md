# Motion sound

Write sound as code on the same timestamps as the picture. Do not drop a music bed on a finished MP4 and hope the transients land.

British English throughout (colour, organised, towards, artefact).

Read `references/render.md` for capture and mux, `references/maths.md` for the beat grid, `references/export.md` for the file X will play.

## Shared timeline

Keep visual cues and audio cues in one JSON list: element, start frame, duration, sound function. If a card transition moves by 12 frames, its click moves by 12 frames. No separate sound edit.

Benchmark tempo for fast SaaS reels: **120 BPM**.

- 2 beats per second
- 1 beat every **30 frames at 60 fps** (0.5 s)
- Hero hits on beats, secondary hits on half-beats (15 frames)

Example grid for a 12-second inbox reel:

| Frame | Beat | Picture | Sound |
| --- | --- | --- | --- |
| 0 | downbeat | first row in | low tick |
| 30 | offbeat | headline holds | rest |
| 60 | downbeat | row starts to open | paper swoosh |
| 90 | offbeat | first reply stacks | short click |
| 120–180 | build | second reply, settle | rising noise, then a drop when the panel resolves |

Place each hit **one frame early** (about 17 ms at 60 fps). The ear leads the eye; a hit on the same frame as the impact feels late.

## Synthesise, do not license

Micro-clicks, paper swooshes, and bass drops are a decaying oscillator plus an envelope. No stock library unless the user supplies music they already own.

Tactile click. Time `t` is seconds since the hit. Keep the envelope short (under 20 ms of useful energy). This is a sample, not a file on disk.

```js
function clickAt(t) {
  const hz = 1650;
  const decay = 110;
  return Math.sin(2 * Math.PI * hz * t) * Math.exp(-t * decay) * 0.35;
}
```

Render the whole bed with `OfflineAudioContext` at 48000 Hz, stereo, length exactly `DURATION` seconds. Live `AudioContext` is not deterministic and will not match the PNG sequence.

Expose `window.renderAudio()` returning a base64 WAV. `capture.mjs` writes `audio.wav` when that function exists.

```js
window.renderAudio = async () => {
  const sampleRate = 48000;
  const ctx = new OfflineAudioContext(2, sampleRate * DURATION, sampleRate);
  for (let beat = 0; beat < DURATION; beat++) {
    const t0 = Math.max(0, beat - 1 / 60);
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    osc.frequency.value = 740;
    gain.gain.setValueAtTime(0.0001, t0);
    gain.gain.exponentialRampToValueAtTime(0.35, t0 + 0.004);
    gain.gain.exponentialRampToValueAtTime(0.0001, t0 + 0.22);
    osc.connect(gain).connect(ctx.destination);
    osc.start(t0);
    osc.stop(t0 + 0.28);
  }
  const buf = await ctx.startRendering();
  return wavBase64(buf);
};

function wavBase64(buf) {
  const channels = buf.numberOfChannels;
  const length = buf.length;
  const sampleRate = buf.sampleRate;
  const bytesPerSample = 2;
  const dataBytes = length * channels * bytesPerSample;
  const view = new DataView(new ArrayBuffer(44 + dataBytes));
  const ascii = (offset, s) => [...s].forEach((c, i) => view.setUint8(offset + i, c.charCodeAt(0)));
  ascii(0, 'RIFF');
  view.setUint32(4, 36 + dataBytes, true);
  ascii(8, 'WAVE');
  ascii(12, 'fmt ');
  view.setUint32(16, 16, true);
  view.setUint16(20, 1, true);
  view.setUint16(22, channels, true);
  view.setUint32(24, sampleRate, true);
  view.setUint32(28, sampleRate * channels * bytesPerSample, true);
  view.setUint16(32, channels * bytesPerSample, true);
  view.setUint16(34, 16, true);
  ascii(36, 'data');
  view.setUint32(40, dataBytes, true);
  let offset = 44;
  for (let i = 0; i < length; i++) {
    for (let c = 0; c < channels; c++) {
      const sample = Math.max(-1, Math.min(1, buf.getChannelData(c)[i]));
      view.setInt16(offset, sample < 0 ? sample * 0x8000 : sample * 0x7fff, true);
      offset += 2;
    }
  }
  let bin = '';
  const bytes = new Uint8Array(view.buffer);
  for (let i = 0; i < bytes.length; i += 0x8000) {
    bin += String.fromCharCode(...bytes.subarray(i, i + 0x8000));
  }
  return btoa(bin);
}
```

`exponentialRampToValueAtTime` cannot take 0. Use `0.0001`. Schedule from `t0 >= 0`.

## Loudness

Aim for about **−14 LUFS** integrated, true peaks under **−1 dBTP**. Common social and streaming target. Check current platform docs if the client has a spec.

```bash
ffmpeg -y -i out/scene.mp4 -i audio.wav -i music.wav -filter_complex \
  "[1:a][2:a]amix=inputs=2:normalize=0,loudnorm=I=-14:TP=-1:LRA=11[a]" \
  -map 0:v -map "[a]" -c:v copy -c:a aac -b:a 192k -ar 48000 -shortest out/final.mp4

ffmpeg -hide_banner -i out/final.mp4 -af ebur128 -f null - 2>&1 | tail -12
```

Single-pass `loudnorm` can land a dB or two off on short, spiky beds. For an exact figure, run it twice: first with `print_format=json` to measure, then again with the measured values.

`-framerate` on the picture must match capture fps or the bed will drift. WAV length must equal `DURATION`. See `references/render.md` troubleshooting.
