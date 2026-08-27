# cairn — releases

Build artifacts for [cairn](https://github.com/Qwertic/cairn). Source lives in a
private repository; this one holds only the compiled binaries, their checksums,
and the installer.

## Install

```sh
curl -fsSL https://raw.githubusercontent.com/Qwertic/cairn-releases/main/install.sh | sh
```

The script detects your platform, downloads the matching binary, **verifies its
SHA256 against `checksums.txt` before installing**, and places it in
`~/.local/bin` — never with `sudo`. If that directory is not on your `PATH` it
prints the exact line to add.

Then:

```sh
cairn init      # connect your agent
cairn doctor    # prove the setup actually works
```

`cairn doctor` is worth running even when nothing looks wrong. It spawns the
exact command your agent is configured to run and completes a real MCP
handshake, which is the only check that proves the setup works where it
actually runs rather than where you tested it.

## Supported platforms

`darwin-arm64` · `darwin-x64` · `linux-x64`

## Pinning a version

```sh
CAIRN_VERSION=v0.2.0 sh install.sh
```

`CAIRN_INSTALL_DIR` overrides where the binary lands.

## Publishing

Releases here are published automatically by the `release` workflow in the
private source repository when a `v*` tag is pushed. Assets are never uploaded
by hand.
