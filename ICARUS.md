# Icarus trial

Upstream source: https://github.com/henriklovhaug/md-tui
Local checkouts: ~/tarballs/md-tui on Logicoma and Icarus.
Upstream license: AGPL-3.0-or-later; retain COPYING/LICENSE and source when distributing.

Build on Icarus with its existing Rust toolchain, without changing system packages:

    cd ~/tarballs/md-tui
    CARGO_PROFILE_RELEASE_LTO=false CARGO_PROFILE_RELEASE_CODEGEN_UNITS=16 \
      nice -n 15 ionice -c 3 ~/.cargo/bin/cargo build \
      --release --locked --no-default-features -j 1

LTO is disabled and code generation split to reduce compiler memory pressure on
this 32-bit, 2 GB laptop. This remains an optimized release build. Optional network
image fetching and programming-language grammar packs are disabled for this offline
reading trial. Local image rendering remains upstream's implementation, not jfbview.

Run `fantasy-mdt` after installation. It starts inside ~/Fantasy/current so the
file index only covers the current collection, not the whole home directory or
all retained editions. Frogmouth and its `fantasy` launcher remain available.

- s: select links, then arrows to choose and Enter to follow.
- b: back to the previous document.
- e: edit in Nano; save/exit to return.
- /: search; t: file browser; q: quit; ?: help; T: theme chooser.

The source is retained for future changes, but the initial trial does not modify
upstream application code. No unit tests or synthetic probes are run; validation
uses release compilation and the actual Fantasy collection on Icarus.

## Installed and checked November 10, 2026

The optimized native build finished successfully in 202m40s. `fantasy-mdt` is
installed in ~/.local/bin and reports mdt 0.11.2. In a separate tmux session on
Icarus, the real collection opened, link selection reached Ice Cold Batou, `e`
opened its file in Nano, Ctrl+X returned to the reader, and `b` returned home.
No dossier content was changed. Physical-console images and jfbview integration
remain unverified; this is the upstream image implementation.
