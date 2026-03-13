# whisper-ja-toggle

Toggle-style voice-to-text using [whisper.cpp](https://github.com/ggerganov/whisper.cpp) on macOS.

1回目の実行で録音開始、2回目で停止→文字起こし→クリップボードにコピー。`loop` モードなら Enter を押すだけでトグルできる。

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
whisper-ja-toggle          # 録音開始
whisper-ja-toggle          # 停止→転記→クリップボードにコピー
```

### Loop mode

ターミナルを1枚開いておけば、Enter だけでトグルできる。

```bash
whisper-ja-toggle loop
```

```
audio device: 2 (cached)
[Enter で録音開始/停止。q で終了]
○ idle >                    # Enter で録音開始
● rec >                     # Enter で停止＆転記
○ idle >                    # q で終了
```

### Subcommands

| Command | Description |
|---------|-------------|
| `toggle` | 録音中なら停止＆転記、停止中なら録音開始（デフォルト） |
| `loop` | インタラクティブモード。Enter でトグル |
| `start` | 録音開始 |
| `stop` | 停止＆転記 |
| `status` | 録音状態を表示 |
| `kill` | 録音プロセスを強制終了 |
| `panic` | 全状態をリセット |

## Configuration

環境変数で動作を変更できる。

| Variable | Default | Description |
|----------|---------|-------------|
| `WHISPER_CPP_HOME` | `~/.local/src/whisper.cpp` | whisper.cpp のパス |
| `WHISPER_MODEL` | `$WHISPER_CPP_HOME/models/ggml-small.bin` | モデルファイル |
| `WHISPER_LANGUAGE` | `ja` | 認識言語 |
| `WHISPER_THREADS` | CPU コア数 | whisper-cli のスレッド数 |
| `WHISPER_AUTOPASTE` | `0` | `1` にすると転記後に自動ペースト |
| `WHISPER_ECHO_TEXT` | `0` | `1` にすると転記テキストを stdout に出力 |
| `WHISPER_KEEP_AUDIO` | `0` | `1` にすると録音ファイルを削除しない |
| `WHISPER_AUDIO_DEVICE_INDEX` | (auto) | ffmpeg の入力デバイスインデックスを固定 |
| `WHISPER_AUDIO_DEVICE_NAME` | (system default) | 入力デバイス名を指定 |
| `WHISPER_DEVICE_CACHE_TTL` | `3600` | デバイスキャッシュの有効秒数 |
| `WHISPER_FFMPEG_BIN` | `ffmpeg` | ffmpeg のパス |
| `WHISPER_STATE_DIR` | `~/Library/Caches/whisper-ja` | 状態ファイルの保存先 |

## Uninstall

```bash
make uninstall
```

## License

MIT
