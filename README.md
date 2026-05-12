# SJinn CLI

[![npm version](https://img.shields.io/npm/v/@sjinn-build/cli.svg)](https://www.npmjs.com/package/@sjinn-build/cli)
[![GitHub release](https://img.shields.io/github/v/release/sjinn-ai/cli)](https://github.com/sjinn-ai/cli/releases)
[![License](https://img.shields.io/badge/license-SJinn%20CLI-blue)](./LICENSE)

Generate images and videos from your terminal with [SJinn AI](https://sjinn.ai).

The CLI supports text-to-image, image-guided generation, text-to-video, image-to-video, async jobs, local downloads, and JSON output for automation.

## Install

```bash
npm install -g @sjinn-build/cli
```

Requirements:

- Node.js 18 or newer
- Network access to npm and GitHub Releases during installation
- macOS, Linux, or Windows on x64/arm64

## Quickstart

Sign in:

```bash
sjinn auth login
sjinn auth whoami
```

Generate an image:

```bash
sjinn image generate --prompt "a cinematic product shot of a glass perfume bottle"
```

Generate a video:

```bash
sjinn video generate --prompt "a slow dolly shot through a neon city street at night"
```

Query an async task:

```bash
sjinn status <task_id>
```

## Examples

### Text to Image

```bash
sjinn image generate \
  --prompt "a studio portrait with soft rim lighting" \
  --model gpt-image-2 \
  --aspect 1:1
```

### Image-Guided Generation

```bash
sjinn image generate \
  --prompt "turn this sketch into a polished product render" \
  --image ./sketch.png \
  --model nano-banana-pro \
  --resolution 2K
```

Use comma-separated values for multiple reference images:

```bash
sjinn image generate \
  --prompt "combine these references into one cohesive fashion campaign image" \
  --image ./pose.png,./fabric.png,https://example.com/mood.jpg
```

### Text to Video

```bash
sjinn video generate \
  --prompt "a luxury watch rotating on black marble" \
  --model veo3 \
  --aspect 16:9
```

### Image to Video

```bash
sjinn video generate \
  --prompt "animate this character with subtle camera movement" \
  --image ./character.png \
  --model kling3 \
  --duration 5
```

### Async Jobs

Return immediately with a task ID:

```bash
sjinn image generate --prompt "a surreal editorial poster" --async
sjinn status <task_id>
```

### Download Results

Save the completed result locally:

```bash
sjinn image generate \
  --prompt "a clean hero image for a design studio" \
  --download ./hero.png

sjinn video generate \
  --prompt "a smooth camera pass over a futuristic workspace" \
  --download ./workspace.mp4
```

### JSON Output

Use JSON output for automation:

```bash
sjinn video generate \
  --prompt "a product demo shot on a white background" \
  --async \
  --json
```

## Commands

| Command | Description |
| --- | --- |
| `sjinn auth login` | Sign in to SJinn. |
| `sjinn auth whoami` | Show the current authenticated user. |
| `sjinn auth logout` | Sign out and clear stored credentials. |
| `sjinn image generate` | Generate an image from a prompt and optional reference images. |
| `sjinn video generate` | Generate a video from a prompt and optional input media. |
| `sjinn status <task_id>` | Query task status. |

Run any command with `--help` for the latest options:

```bash
sjinn --help
sjinn auth --help
sjinn image generate --help
sjinn video generate --help
```

`sjinn compose` is reserved for upcoming media composition features.

## Image Options

```bash
sjinn image generate [options]
```

| Option | Description |
| --- | --- |
| `--prompt <text>` | Image description. |
| `--model <name>` | Image model. Defaults to `gpt-image-2`. |
| `--image <paths-or-urls>` | Comma-separated local paths or URLs for reference images. |
| `--aspect <ratio>` | Aspect ratio, such as `1:1`, `16:9`, or `9:16`. |
| `--resolution <res>` | Resolution, such as `1K`, `2K`, or `4K`, where supported. |
| `--async` | Return the task ID without waiting. |
| `--download [path]` | Download the result to a local file. |
| `--json` | Print machine-readable JSON. |

## Video Options

```bash
sjinn video generate [options]
```

| Option | Description |
| --- | --- |
| `--prompt <text>` | Video description. |
| `--model <name>` | Video model. Defaults to `veo3`. |
| `--image <path-or-url>` | Input image for image-to-video. |
| `--duration <seconds>` | Video duration in seconds, where supported. |
| `--aspect <ratio>` | Aspect ratio, such as `16:9`, `9:16`, or `1:1`. |
| `--mode <mode>` | Quality mode, such as `std`, `pro`, `fast`, or `standard`, where supported. |
| `--resolution <res>` | Output resolution, such as `480p`, `720p`, or `1080p`, where supported. |
| `--end-image <path-or-url>` | End frame image, where supported. |
| `--media-urls <urls>` | Comma-separated reference media URLs, where supported. |
| `--multi-shot <bool>` | Multi-shot mode, where supported. |
| `--async` | Return the task ID without waiting. |
| `--download [path]` | Download the result to a local file. |
| `--json` | Print machine-readable JSON. |

## Models

### Image Models

| Model | Notes |
| --- | --- |
| `gpt-image-2` | Default image model. Supports `--image`; `--aspect` `auto`, `1:1`, `16:9`, `9:16`, `3:2`, `2:3`. |
| `nano-banana` | Supports `--image`; `--aspect` `auto`, `1:1`, `16:9`, `9:16`, `4:3`, `3:4`, `9:21`, `21:9`, `2:3`, `3:2`. |
| `nano-banana-pro` | Supports `--image`; `--aspect` same as `nano-banana`; `--resolution` `1K`, `2K`. |
| `nano-banana-2` | Supports `--image`; `--aspect` same as `nano-banana`; `--resolution` `1K`, `2K`, `4K`. |
| `seedream` | Supports `--image`; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:2`, `2:3`, `3:4`. |
| `seedream-v4` | Supports `--image`; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:2`, `2:3`, `3:4`. |

### Video Models

| Model | Notes |
| --- | --- |
| `veo3` | Default video model. Supports `--image`; `--aspect` `16:9`, `9:16`; `--end-image` for image-to-video. |
| `sora2` | Supports `--image`; `--aspect` `16:9`, `9:16`; `--duration` `4`, `8`, `12`; `--mode` `std`, `pro`. |
| `grok` | Supports `--image`; `--aspect` `16:9`, `9:16`, `1:1`; `--duration` 3-15 seconds. |
| `kling3` | Supports `--image`; text-to-video `--aspect` `16:9`, `9:16`, `1:1`; `--duration` 3-15 seconds; `--mode` `standard`, `pro`; `--multi-shot` `true`, `false`; `--end-image` for image-to-video. |
| `seedance2` | Supports `--image`, `--media-urls` up to 9 items; `--aspect` `16:9`, `9:16`, `1:1`, `4:3`, `3:4`; `--duration` 4-15 seconds; `--mode` `pro`, `fast`; `--resolution` `480p`, `720p`, `1080p`. |

## Updating

```bash
npm install -g @sjinn-build/cli@latest
sjinn --version
```

## Uninstall

```bash
npm uninstall -g @sjinn-build/cli
```

## How Installation Works

The npm package installs a small JavaScript wrapper. During `postinstall`, it downloads the matching prebuilt `sjinn` binary from the public [sjinn-ai/cli GitHub Releases](https://github.com/sjinn-ai/cli/releases) page, verifies its SHA256 checksum, and stores it in the package's `vendor/` directory.

## Manual Binary Download

If npm installation cannot reach GitHub Releases, download the matching asset manually from [sjinn-ai/cli releases](https://github.com/sjinn-ai/cli/releases):

| Platform | Asset |
| --- | --- |
| macOS Apple Silicon | `sjinn-vX.Y.Z-darwin-arm64.tar.gz` |
| macOS Intel | `sjinn-vX.Y.Z-darwin-amd64.tar.gz` |
| Linux x64 | `sjinn-vX.Y.Z-linux-amd64.tar.gz` |
| Linux arm64 | `sjinn-vX.Y.Z-linux-arm64.tar.gz` |
| Windows x64 | `sjinn-vX.Y.Z-windows-amd64.zip` |
| Windows arm64 | `sjinn-vX.Y.Z-windows-arm64.zip` |

Use the matching `sjinn-vX.Y.Z-checksums.txt` file to verify the asset before running it.

## Troubleshooting

### Installation Cannot Download the Binary

Confirm the machine can access:

```text
https://github.com/sjinn-ai/cli/releases
```

Then retry:

```bash
npm install -g @sjinn-build/cli@latest
```

### Checksum Mismatch

Delete the failed installation and reinstall. A checksum mismatch usually means the download was interrupted or corrupted.

```bash
npm uninstall -g @sjinn-build/cli
npm install -g @sjinn-build/cli@latest
```

### Unsupported Platform

Prebuilt binaries are published for macOS, Linux, and Windows on x64/arm64. Other platforms are not supported by the npm installer.

## Support

Open an issue in [sjinn-ai/cli issues](https://github.com/sjinn-ai/cli/issues) and include:

- `sjinn --version`
- `node --version`
- Operating system and CPU architecture
- The command that failed
- Any error output

## License

This package is distributed under the SJinn CLI License. See [LICENSE](./LICENSE).
