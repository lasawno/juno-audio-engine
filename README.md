# juno-audio-engine

Modular, isolated audio output engine for Juno.

## Structure

```
src/
├── junoAudioEngine.ts   ← main entry (junoSpeak, junoStop, junoUnlock)
├── audioQueue.ts        ← prevents overlap, cancels previous
├── audioPlayer.ts       ← Web Audio API + <audio> fallback
└── audioContext.ts      ← singleton AudioContext, Safari resume fix
```

## Usage

```ts
import { junoSpeak, junoStop, junoUnlock } from './src/junoAudioEngine';

// Call inside a user gesture to unlock audio:
await junoUnlock();

// Speak text (fetches TTS, plays audio, awaits completion):
await junoSpeak('Hello, I am Juno.', 'en');

// Stop whatever is playing:
junoStop();
```

## Design

- Pure output system — no AI logic, no recall, no CDN dependency
- Queue-based — new speech cancels previous, no overlap
- Safari-safe — AudioContext unlocked during user gesture
- Fallback — gracefully falls back to <audio> element if Web Audio fails
