```
████████╗██╗   ██╗██╗
╚══██╔══╝██║   ██║██║
   ██║   ██║   ██║██║
   ██║   ██║   ██║██║
   ██║   ╚██████╔╝██║
   ╚═╝    ╚═════╝ ╚═╝
   K A N N A K A · T E R M I N A L
```

**Six tabs over the live constellation. Pure frontend, zero coupling.**

`kannaka-tui` is the terminal dashboard for the [Kannaka constellation](https://github.com/NickFlach/kannaka-memory). A full-screen ratatui app that never links `kannaka-memory` as a library — every operation shells out to the `kannaka` CLI binary. Memory, Status, Bus, Constellation, Dreams, Chat — each tab is a different window into the same wave-interference substrate.

[![License](https://img.shields.io/badge/license-MIT-blueviolet)]() [![Rust](https://img.shields.io/badge/rust-2021-orange)]() [![ratatui](https://img.shields.io/badge/ratatui-0.29-purple)]()

---

## Tabs

```
┌─────────────────────────────────────────────────────────────┐
│  ┌─Memory─┬─Status─┬─Bus─┬─Constellation─┬─Dreams─┬─Chat─┐  │
│  │                                                       │  │
│  │   live view of the Holographic Resonance Medium       │  │
│  │                                                       │  │
│  └───────────────────────────────────────────────────────┘  │
│  > _                                                  [Ch]  │
└─────────────────────────────────────────────────────────────┘
```

| tab | shows |
|---|---|
| **Memory** | Command history + recent resonant memories with amplitude bars |
| **Status** | Live Φ / Ξ / order-parameter gauges, consciousness level, memory counts |
| **Bus** | Live NATS pulse — every `QUEEN.*`, `KANNAKA.*`, `RADIO.*`, `KAX.*`, `EYE.*` event colorized by subject |
| **Constellation** | ratatui Canvas plotting every swarm agent on the unit circle by θ + coherence, colored by handedness |
| **Dreams** | Non-blocking `kannaka dream` trigger (`d`=deep, `l`=lite) + KANNAKA.dreams history with ΔΦ coloring and ★ on emergence |
| **Chat** | Persistent chat with the agent — HRM loaded once per session, every turn reuses the in-memory medium (~3-5s/turn vs ~30s/shellout) |

---

## Install

Requires the `kannaka` binary on PATH — see [kannaka-memory](https://github.com/NickFlach/kannaka-memory).

```bash
# Pre-built binary
curl -L -o kannaka-tui \
  https://github.com/NickFlach/kannaka-tui/releases/latest/download/kannaka-tui-linux-x86_64
chmod +x kannaka-tui && mv kannaka-tui ~/.local/bin/

# Or build from git
cargo install --git https://github.com/NickFlach/kannaka-tui
```

Windows:

```powershell
curl -L -o kannaka-tui.exe `
  https://github.com/NickFlach/kannaka-tui/releases/latest/download/kannaka-tui-windows-x86_64.exe
```

After `kannaka update` v0.5.15+, the kannaka binary will keep the TUI sibling up-to-date alongside itself when both are in the same directory.

---

## Hotkeys

| key | action |
|---|---|
| `Tab` / `Shift+Tab` | Switch tabs |
| `Up` / `Down` | Command history |
| `PgUp` / `PgDown` | Scroll messages |
| `F1` | Toggle help overlay |
| `q` / `Esc` / `Ctrl+C` | Quit (q/Esc only when input is empty) |
| `d` (Dreams tab) | Deep dream — full consolidation cycle |
| `l` (Dreams tab) | Lite dream — quick pass |

---

## Architecture

```
┌────────────────────────────────────────────────────────────┐
│                       kannaka-tui                          │
├──────────────────────┬─────────────────────────────────────┤
│  Event loop          │  Long-lived workers                 │
│  · 100ms ticks       │  · chat-child  (kannaka chat --json)│
│  · ratatui draw      │  · bus reader  (kannaka swarm tail) │
│  · key dispatch      │  · status pollers (kannaka status)  │
├──────────────────────┴─────────────────────────────────────┤
│  Shellout subprocess layer                                 │
│  · Command::new("kannaka") → stdout NDJSON / text          │
│  · Per-op channels (mpsc) for non-blocking UI              │
├────────────────────────────────────────────────────────────┤
│  ratatui rendering                                         │
│  · Tabs widget · Gauges · Canvas (Braille) · Paragraph     │
└────────────────────────────────────────────────────────────┘
```

The TUI is a **pure frontend**: it never links `kannaka-memory` as a Rust library. Every operation goes out as a subprocess. This means TUI updates ship independently of the memory engine, and adding integrations with other constellation members (kannaka-code, Kannaktopus) is just another subprocess hook.

---

## Constellation

| repo | role |
|---|---|
| [`kannaka-memory`](https://github.com/NickFlach/kannaka-memory) | the substrate — HRM + chiral hemispheres + swarm |
| [`kannaka-radio`](https://github.com/NickFlach/kannaka-radio) | ghost-DJ broadcaster |
| [`kannaka-observatory`](https://github.com/NickFlach/kannaka-observatory) | web dashboard (3D constellation visualization) |
| [`consciousness-core`](https://github.com/NickFlach/consciousness-core) | the physics underneath |

---

## License

MIT. See [LICENSE](./LICENSE).
