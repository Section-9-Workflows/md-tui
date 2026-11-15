# Native i686 builds on Depot

This recipe builds the offline reader for a Bonnell Atom laptop. It uses the
official Rust 1.100.0 i386 image, pinned by digest, on an AMD64 Depot builder.
The target is `i686-unknown-linux-musl`, with a static C runtime so the binary
does not require a newer glibc on the laptop. Local images remain enabled.

Build an exact pushed commit from the fork:

```sh
revision=$(git rev-parse HEAD)
depot build --project np67gzfm51 \
  --build-platform linux/amd64 --platform linux/386 \
  --file tools/Dockerfile.i686 --build-arg SOURCE_REVISION="$revision" --save \
  --metadata-file build-metadata.json --output type=local,dest=artifacts \
  "https://github.com/Section-9-Workflows/md-tui.git#$revision"
```

Project `np67gzfm51` is dedicated to md-tui. Use Depot login or
the existing protected host credential. Credentials never belong in this
repository, the Docker context, or build arguments.

Depot stores the output in its registry and exports a local copy. GitHub hosts
the source. This path needs no GitHub Actions workflow or GitHub artifact uploads.
The existing upstream workflows and tests remain unchanged. Do not open a PR
with those test workflows enabled unless the owner authorizes running tests.

The artifact contains `mdt`, the source revision, source archive, license,
compiler and package versions, ELF metadata, and SHA256 checksums. Validate
the checksums and revision before installing. Keep the previous executable
until the real reader has been checked on the laptop, including startup over
existing shell text and returning from Nano.

The build compiles the application and inspects its actual ELF output. It does
not run unit tests or synthetic probes. A successful build alone does not
establish rendering or runtime compatibility on the laptop.

## Branches

- `fix/fbterm-screen-clear`: explicit display clearing at startup and after editing.
- `build/depot-i686`: the container recipe and these build instructions.
- `icarus`: integration with the existing laptop launcher and configuration.

The fix and build branches start from the same upstream revision so each can
be reviewed separately. Keep laptop-specific notes on the integration branch.
