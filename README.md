# whisper-ja-toggle

Toggle-style voice-to-text using [whisper.cpp](https://github.com/ggerganov/whisper.cpp) on macOS.

Run once to start recording, run again to stop, transcribe, and copy to clipboard. Everything runs locally on your machine — no audio data is sent over the network.

Use `loop` mode to keep a terminal open and toggle recording with just Enter.

## Requirements

- macOS
- [whisper.cpp](https://github.com/ggerganov/whisper.cpp) (built with `whisper-cli`)
- [ffmpeg](https://ffmpeg.org/)
- A whisper.cpp model (default: `ggml-small.bin`)

### Setup whisper.cpp

```bash
git clone https://github.com/ggerganov/whisper.cpp.git ~/.local/src/whisper.cpp
cd ~/.local/src/whisper.cpp
cmake -B build
cmake --build build --config Release
./models/download-ggml-model.sh small
```

### Setup ffmpeg

```bash
brew install ffmpeg
```

## Install

```bash
git clone https://github.com/chaspy/whisper-ja-toggle.git
cd whisper-ja-toggle
make install
```

Installs to `~/.local/bin/` by default. Change with `PREFIX`:

```bash
make install PREFIX=/usr/local
```

## Usage

### Toggle mode (default)

```bash
whisper-ja-toggle          # start recording
whisper-ja-toggle          # stop → transcribe → copy to clipboard
```

### Loop mode

Keep one terminal open and press Enter to toggle recording on and off.

```bash
whisper-ja-toggle loop
```

```
audio device: 2 (cached)
[Press Enter to start/stop recording. Type q to quit.]
○ idle >                    # press Enter to start recording
● rec >                     # press Enter to stop and transcribe
○ idle >                    # type q to quit
```

### Subcommands

| Command | Description |
|---------|-------------|
| `toggle` | Start or stop recording (default) |
| `loop` | Interactive mode — toggle with Enter |
| `start` | Start recording |
| `stop` | Stop recording and transcribe |
| `status` | Show recording state |
| `kill` | Force-stop the recording process |
| `panic` | Reset all state |

## Privacy

All processing happens locally. Audio is recorded to a temporary file, transcribed by whisper.cpp on your CPU, and the result is copied to the clipboard. No data leaves your machine.

## Configuration

All settings are optional and configured via environment variables.

| Variable | Default | Description |
|----------|---------|-------------|
| `WHISPER_CPP_HOME` | `~/.local/src/whisper.cpp` | Path to whisper.cpp |
| `WHISPER_MODEL` | `$WHISPER_CPP_HOME/models/ggml-small.bin` | Model file |
| `WHISPER_LANGUAGE` | `ja` | Recognition language |
| `WHISPER_THREADS` | Number of CPU cores | Thread count for whisper-cli |
| `WHISPER_AUTOPASTE` | `0` | Set to `1` to auto-paste after transcription |
| `WHISPER_ECHO_TEXT` | `0` | Set to `1` to print transcribed text to stdout |
| `WHISPER_KEEP_AUDIO` | `0` | Set to `1` to keep the recorded audio file |
| `WHISPER_AUDIO_DEVICE_INDEX` | (auto-detected) | Fix the ffmpeg input device index |
| `WHISPER_AUDIO_DEVICE_NAME` | (system default) | Specify input device by name |
| `WHISPER_DEVICE_CACHE_TTL` | `3600` | Device cache TTL in seconds |
| `WHISPER_FFMPEG_BIN` | `ffmpeg` | Path to ffmpeg |
| `WHISPER_STATE_DIR` | `~/Library/Caches/whisper-ja` | Directory for state files |

## Uninstall

```bash
make uninstall
```

## License

MIT
