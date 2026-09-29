# Whisper

**Whisper base (speech recognition)**, as a sherpa-onnx bundle — Robust multilingual speech-to-text.

An **azphalt** AI-model plugin, packaged as a `.azp` (the azphalt analogue of a VS Code `.vsix`). It is
named for the *model*, not a single feature — the same model powers many tools, and it is **host-neutral**:
any azphalt host that understands its role can use it, not just one app. Install it from any host's
**Azphalt Storefront**.

## What it can do

- transcription
- auto-captions / subtitles
- translate-to-English
- searchable spoken content

## Roles (host-neutral routing)

This plugin contributes the role(s): `speech-to-text`. A host routes the model by role — it carries no
`targetApps`, so it is not tied to any single application.

**Example host — [Guillotine](https://github.com/HereLiesAz/Guillotine):** Desktop `asrModelPath` — precise transcription.

## Model file(s)

- **`whisper-base.zip`** (role `speech-to-text`, type `sherpa-bundle`) — `base-encoder.int8.onnx`, `base-decoder.int8.onnx`,
  `base-tokens.txt`, repackaged flat from [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/sherpa-onnx-whisper-base.tar.bz2).
  Hosts extract it into its own folder (`whisper-base/`).

Model license: **MIT (Whisper, OpenAI)**. This plugin's manifest/packaging is `MIT`.

## How it works — the VSCode Header Pattern

The `.azp` does **not** bundle the weights. The manifest declares each model as a *remote asset*
(`"path": ""` + `remoteUrl` + `checksum` + `byteSize`); the host downloads the weights on install and
verifies them against the pinned SHA-256 — exactly how a large VS Code extension fetches its language
server instead of shipping it inside the `.vsix`. `remoteUrl` points at this repo's own GitHub **Release**
asset (named the exact filename the host expects); the `release` workflow fetches the upstream model,
repackages it as a zip, checksums it, and publishes it beside the packed `.azp`.

## Build / release

~~~sh
npm install && npm run build     # packs com.hereliesaz.azphalt.whisper-1.1.0.azp
git tag v1.1.0 && git push --tags   # runs the release workflow: hosts the model + .azp
~~~
