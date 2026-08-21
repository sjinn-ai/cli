# SJinn CLI

[![npm version](https://img.shields.io/npm/v/@sjinn-build/cli.svg)](https://www.npmjs.com/package/@sjinn-build/cli)
[![GitHub release](https://img.shields.io/github/v/release/sjinn-ai/cli)](https://github.com/sjinn-ai/cli/releases)
[![License](https://img.shields.io/badge/license-SJinn%20CLI-blue)](./LICENSE)

Generate images, videos, and audio from your terminal with [SJinn AI](https://sjinn.ai). The CLI supports reference media, async jobs, local downloads, JSON output, and video composition.

## Install

```bash
npm install -g @sjinn-build/cli
```

Requirements:

- Node.js 18 or newer
- Network access to npm and GitHub Releases during installation
- macOS, Linux, or Windows on x64/arm64

## Quickstart

Authenticate in a browser, or save an API token directly:

```bash
sjinn auth login
sjinn auth login --token <your-token>
sjinn auth whoami
```

For automation, provide the token through `SJINN_TOKEN`:

```bash
SJINN_TOKEN=<your-token> sjinn image generate --prompt "a cat in space"
```

Generate media:

```bash
sjinn image generate --prompt "a cinematic product shot of a glass perfume bottle"
sjinn video generate --prompt "a slow dolly shot through a neon city street at night"
sjinn audio generate --prompt "a short upbeat intro sound"
```

Use `--async` to return a task ID immediately, then query it with `sjinn status <task_id>`. Add `--json` for machine-readable output, or `--download [path]` to save a completed result locally.

## Commands

| Command | Description |
| --- | --- |
| `sjinn auth login [--token <token>]` | Authenticate in a browser or save an API token. |
| `sjinn auth whoami` | Verify the current credentials and show the API endpoint. |
| `sjinn auth logout` | Clear stored credentials. |
| `sjinn image generate` | Generate an image from a prompt and optional reference images. |
| `sjinn video generate` | Generate a video from a prompt and optional input media. |
| `sjinn audio generate` | Generate audio from a prompt and optional reference media. |
| `sjinn compose` | Concatenate videos in order. |
| `sjinn status <task_id>` | Query task status. |

Run `sjinn --help` or any command with `--help` for the installed CLI's option help.

## Common Options

All generation commands require `--prompt <text>` and support `--model <name>`, `--async`, `--download [path]`, and `--json`.

| Command | Additional options |
| --- | --- |
| `image generate` | `--image <paths-or-urls>`, `--aspect <ratio>`, `--resolution <res>` |
| `video generate` | `--image <path-or-url>`, `--duration <seconds>`, `--aspect <ratio>`, `--mode <mode>`, `--resolution <res>`, `--end-image <path-or-url>`, `--image-urls <items>`, `--video-urls <items>`, `--media-urls <items>`, `--multi-shot <bool>` |
| `audio generate` | `--audio-urls <items>`, `--image-urls <item>` |
| `compose` | `--video-urls <paths-or-urls>`, `--async`, `--download [path]`, `--json` |

### Examples

```bash
# Image with local or HTTPS reference images
sjinn image generate --prompt "polished product render" --image ./sketch.png,https://example.com/mood.jpg --model nano-banana-pro --resolution 2K

# Image-to-video
sjinn video generate --prompt "animate this character" --image ./character.png --model kling3 --duration 5

# Gemini Omni Video references
sjinn video generate --prompt "two characters in a lab" --model gemini-omni-video --image-urls ./a.png,./b.png --video-urls ./reference.mov --duration 6

# Seedance 2.0 multi-reference video
sjinn video generate --prompt "two characters in a lab" --model seedance2 --media-urls ./character-a.png,./character-b.png,./lab.mp4 --duration 5 --json

# Seedance 2.5 multi-reference video
sjinn video generate --prompt "a concert teaser" --model seedance2.5 --image-urls ./artist.png,./stage.png --video-urls ./movement.mp4 --audio-urls ./track.wav --aspect 16:9 --duration 8 --resolution 1080p

# Seed Audio 1.0 reference audio
sjinn audio generate --prompt "narrate in the style of @Audio1" --audio-urls ./ref-a.mp3 --model seed-audio-1-0

# Compose at least two videos in playback order
sjinn compose --video-urls "./intro.mp4,./outro.mov" --download ./combined.mp4
```

## Models and Limits

### Image models

| Model | Supported parameters |
| --- | --- |
| `gpt-image-2` (default) | `--image`; `--aspect` `auto`, `1:1`, `16:9`, `9:16`, `3:2`, `2:3` |
| `nano-banana` | `--image`; `--aspect` `auto`, `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `9:21`, `21:9`, `2:3`, `3:2` |
| `nano-banana-pro` | `nano-banana` aspects; `--resolution` `1K`, `2K` |
| `nano-banana-2` | `nano-banana` aspects; `--resolution` `1K`, `2K`, `4K` |
| `seedream` | `--image`; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:2`, `2:3`, `3:4` |
| `seedream-v4` | `--image`; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:2`, `2:3`, `3:4` |

### Video models

| Model | Supported parameters |
| --- | --- |
| `veo3` (default) | `--image`; `--aspect` `16:9`, `9:16`; `--end-image` for image-to-video. |
| `gemini-omni-video` | `--image-urls` up to 7 reference images, `--video-urls` optional reference video (at most 1); `--aspect` `16:9`, `9:16`; `--duration` `4`, `6`, `8`, `10` seconds. |
| `grok` | `--image`; `--aspect` `16:9`, `9:16`, `1:1`; `--duration` 3-15 seconds. |
| `kling3` | `--image`; text-to-video `--aspect` `16:9`, `9:16`, `1:1`; `--duration` 3-15 seconds; `--mode` `standard`, `pro`; `--multi-shot` `true`, `false`; `--end-image` for image-to-video. |
| `seedance2` | `--image`, `--media-urls` up to 9 total image/video/audio reference paths or URLs; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:4`; `--duration` 4-15 seconds; `--mode` `pro`, `fast`; `--resolution` `480p`, `720p`, `1080p`. |
| `seedance2.5` | `--image-urls` up to 30 reference images; `--video-urls` up to 10 reference videos; `--audio-urls` up to 10 reference audios; `--aspect` `16:9`, `9:16`, `1:1`, `3:4`, `4:3`, `21:9`; `--duration` 4-30 seconds; `--resolution` `480p`, `720p`, `1080p`. |

For Seedance 2.0, `--image` and `--media-urls` are mutually exclusive. Put all multi-reference inputs in `--media-urls`. Local `--media-urls` files are limited to `.jpg`, `.jpeg`, `.png`, `.webp`, `.mp4`, `.mov`, `.mp3`, `.m4a`, and `.wav`.

Seedance 2.5 local reference images support `.jpg`, `.jpeg`, `.png`, and `.webp`; videos support `.mp4` and `.mov`; audios support `.mp3`, `.m4a`, and `.wav`. `--media-urls` is only available for Seedance 2.0.

### Audio model

| Model | Supported parameters |
| --- | --- |
| `seed-audio-1-0` (default) | Up to 3 `--audio-urls` references, cited as `@Audio1`, `@Audio2`, `@Audio3`, or 1 `--image-urls` reference |

`--audio-urls` and `--image-urls` are mutually exclusive. Local audio references are limited to `.mp3`, `.m4a`, and `.wav`; local image references are limited to `.jpg`, `.jpeg`, `.png`, and `.webp`.

## Local Files and Composition

Local paths are uploaded automatically; HTTPS URLs are passed through. Image generation accepts local upload types `.jpg`, `.jpeg`, `.png`, `.webp`, `.mp4`, `.mov`, `.mp3`, `.m4a`, `.wav`, `.zip`, and `.glb`; model-specific reference options apply the stricter limits above.

`sjinn compose` is available now. It requires at least two ordered videos. Local compose inputs must be `.mp4` or `.mov`; HTTPS URLs are accepted. Separate audio, voiceover, background music, and transitions are not supported.

## Configuration

The config file is `~/.sjinn/config.json`. `SJINN_TOKEN` supplies the API token, and `SJINN_API_BASE` overrides the API base URL (default: `https://sjinn.ai`).

Configuration resolution is: **CLI flags > environment variables > config file > default**. `sjinn auth login --token <token>` saves the token to the config file; `SJINN_TOKEN` takes precedence over that saved token for automated runs.

## Updating and Uninstalling

```bash
npm install -g @sjinn-build/cli@latest
sjinn --version

npm uninstall -g @sjinn-build/cli
```

## How Installation Works

The npm package installs a small JavaScript wrapper. During `postinstall`, it downloads the matching prebuilt `sjinn` binary from [sjinn-ai/cli GitHub Releases](https://github.com/sjinn-ai/cli/releases), verifies its SHA256 checksum, and stores it in the package's `vendor/` directory.

## Manual Binary Download

If npm installation cannot reach GitHub Releases, download the matching asset manually from [sjinn-ai/cli releases](https://github.com/sjinn-ai/cli/releases).

| Platform | Asset |
| --- | --- |
| macOS Apple Silicon | `sjinn-vX.Y.Z-darwin-arm64.tar.gz` |
| macOS Intel | `sjinn-vX.Y.Z-darwin-amd64.tar.gz` |
| Linux x64 | `sjinn-vX.Y.Z-linux-amd64.tar.gz` |
| Linux arm64 | `sjinn-vX.Y.Z-linux-arm64.tar.gz` |
| Windows x64 | `sjinn-vX.Y.Z-windows-amd64.zip` |
| Windows arm64 | `sjinn-vX.Y.Z-windows-arm64.zip` |

Verify the downloaded asset with the matching `sjinn-vX.Y.Z-checksums.txt` file before running it.

## Troubleshooting and Support

### Installation cannot download the binary

Confirm access to `https://github.com/sjinn-ai/cli/releases`, then retry:

```bash
npm install -g @sjinn-build/cli@latest
```

### Checksum mismatch

The download may have been interrupted or corrupted. Reinstall the package:

```bash
npm uninstall -g @sjinn-build/cli
npm install -g @sjinn-build/cli@latest
```

### Authentication or API errors

Run `sjinn auth whoami` to check stored credentials. Reauthenticate with `sjinn auth login` or provide a valid `SJINN_TOKEN`. Confirm `SJINN_API_BASE` points to the intended SJinn endpoint if you set it.

### Unsupported local file or invalid option

Check the model limits and extension restrictions above, and run the relevant `--help` command. Local extensions are validated before upload; remote HTTP(S) URLs are passed through where the option accepts URLs.

### Unsupported platform

The npm installer publishes binaries only for macOS, Linux, and Windows on x64/arm64.

For help, open an issue in [sjinn-ai/cli issues](https://github.com/sjinn-ai/cli/issues) with `sjinn --version`, `node --version`, your OS and CPU architecture, the failing command, and its error output.

## License

This package is distributed under the SJinn CLI License. See [LICENSE](./LICENSE).
