# Offline build

The `offline` cargo feature builds the OSS app (`warp-oss`) so it runs with no
Warp account and never talks to Warp's servers.

## What it changes

- Every Warp server URL (`app.warp.dev`, `rtc.app.warp.dev`, session sharing,
  Oz) points at a closed local port (`127.0.0.1:9`), so anything that slips
  through fails immediately on the machine. See `app/src/bin/oss.rs`.
- The login screen and agent onboarding are skipped; the app opens straight to
  the terminal. Because there is no account, Warp's hosted AI, Warp Drive and
  settings sync stay off.
- Feature flags that need Warp's servers are turned off: hosted AI (Agent
  Mode), cloud objects, shared sessions, auto-update, changelog, crash
  reporting and the SSH remote-server download. See `app/src/features.rs`.
- The OSS channel already ships with telemetry, crash reporting and
  auto-update disabled.

Settings, themes, workflows and history are stored locally, under `~/.warp-oss`
and the app's local data directories.

## Using a local or on-prem model

Warp's built-in agent runs on Warp's servers, so it is not available here. Run
any CLI coding agent in the terminal instead and point it at your model's
OpenAI-compatible endpoint (for example an Ollama or vLLM server on your
network).

## Build and run on Linux

```bash
./script/linux/install_build_deps
./script/linux/install_runtime_deps
cargo run -p warp --bin warp-oss --features gui,offline
```

To produce packages (`.deb`, `.rpm`, AppImage):

```bash
./script/linux/bundle --channel oss --features offline
```
