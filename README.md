# shuma

> An interactive shell with zsh/fish parity — multiplexer and remote sessions built in — in Rust, on [Llimphi](https://github.com/tawasuyu/llimphi).

`shuma` replaces `zsh + tmux + mosh` with a single piece: a shell with history, completion and job-control, **native multiplexing** (no tmux), **remote sessions** (no mosh, no SSH daemon), all inside a Llimphi 4-slot chassis (TopBar · Main · BottomBar · Quake drawer). Optional `intent → command` inference layers an LLM on top — without it, the traditional shell runs unchanged. `matilda` is the sibling tool for declarative multi-host configuration.

<p align="center">
  <img src="docs/shuma_showreel.gif" alt="shuma showreel — a block-based terminal: a sortable ls -l table, collapsible ls -R sub-blocks, git status sections, a folded git log, and a live-streaming cargo build" width="900">
  <br>
  <sub>a block-based terminal — sortable tables, collapsible output, detected sections, live streaming</sub>
</p>

## Run

```sh
cargo run --release -p shuma-shell-llimphi   # the GUI shell (Llimphi chassis)
cargo run --release -p shuma-cli             # CLI front-end
cargo run --release -p shuma-daemon          # session daemon (local + remote)
```

## Architecture

- **`shuma-core`** — shell engine: parsing, history, completion, job control. UI-agnostic.
- **`shuma-shell-llimphi`** — the GPU chassis (4 slots + Quake drawer + command bar + launcher modules).
- **`shuma-protocol` / `shuma-remote-exec`** — local-client ↔ remote-server over TCP/TLS, no SSH daemon required.
- **`matilda-*`** — declarative multi-host config (the bare-metal sibling).

## How dependencies work

Clean front-door repo: it contains **only shuma's own crates**. Llimphi and every foundational dependency (content-addressed cards, P2P discovery via `chasqui`/`minga`, the `pata` panel host, `arje` process primitives, shared leaves) are pulled as git dependencies from the [`tawasuyu`](https://github.com/tawasuyu/tawasuyu) monorepo — the suite's source of truth. No vendoring, no duplication; the first build clones the monorepo (cached afterwards).

## Considerations

- **Replacement, not addition.** With shuma you can drop zsh/tmux/mosh — the behavior is covered.
- Remote sessions use `shuma-protocol`, not an SSH daemon.
- Cross-platform Llimphi UI; also runs inside the Wawa kernel.

## License

MIT. Builds on [Llimphi](https://github.com/tawasuyu/llimphi) and the [tawasuyu](https://github.com/tawasuyu/tawasuyu) suite.
