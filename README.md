# kannaka-tui

Terminal dashboard for the [Kannaka constellation](https://github.com/NickFlach/kannaka-memory).
A pure-frontend ratatui app over the `kannaka` CLI — six tabs covering the
constellation's live state, with non-blocking workers so the UI stays
responsive while the holographic medium does its slow work.

## Tabs

| tab | what it shows |
|---|---|
| **Memory** | Command history + recent resonant memories with amplitude bars |
| **Status** | Live Φ / Ξ / order-parameter gauges, consciousness level, memory counts |
| **Bus** | Live NATS pulse — every `QUEEN.*`, `KANNAKA.*`, `RADIO.*`, `KAX.*`, `EYE.*` event, colorized by subject |
| **Constellation** | ratatui Canvas plotting every swarm agent on the unit circle by θ + coherence, colored by handedness |
| **Dreams** | Non-blocking `kannaka dream` trigger (d=deep, l=lite) + KANNAKA.dreams history with ΔΦ coloring and ★ on emergence |
| **Chat** | Persistent chat with the agent — HRM loaded once, every turn reuses the loaded medium |

## Install

Requires the `kannaka` binary on PATH — see [kannaka-memory](https://github.com/NickFlach/kannaka-memory).

```bash
cargo install kannaka-tui
```

Or grab a release binary from the [releases page](https://github.com/NickFlach/kannaka-tui/releases).

## Hotkeys

| key | action |
|---|---|
| `Tab` / `Shift+Tab` | Switch tabs |
| `Up` / `Down` | Command history |
| `PgUp` / `PgDown` | Scroll messages |
| `F1` | Toggle help overlay |
| `q` / `Esc` / `Ctrl+C` | Quit (q/Esc only when input is empty) |
| `d` (Dreams tab) | Deep dream — full consolidation cycle (~30s) |
| `l` (Dreams tab) | Lite dream — quick pass |

## Architecture

`kannaka-tui` is a **pure frontend**. It never links `kannaka-memory` as a
library — every operation shells out to the `kannaka` binary via subprocess.

- One-shot ops (`remember`, `recall`, `status`, `observe`, `dream`) spawn
  `kannaka <verb>` and parse stdout
- Chat is backed by a long-running `kannaka chat --json` child so the HRM
  loads once and every turn reuses the in-memory medium
- The Bus / Constellation tabs spawn `kannaka swarm tail` and parse its
  NDJSON stdout, with one mpsc channel for the bus log and a parallel one
  for parsed per-agent snapshots

This separation lets `kannaka-memory` evolve as a library + CLI without
the TUI riding along on every release, and lets the TUI grow integrations
with siblings like `kannaka-code` and `Kannaktopus` without bloating the
memory engine.

## License

MIT. See [LICENSE](./LICENSE).
