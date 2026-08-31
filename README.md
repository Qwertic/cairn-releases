# cairn

**Shared team knowledge for AI agents — they propose what they learn, a human
approves what lands.**

Your coding agents discover things all day: why a service is built the way it is,
which gotcha cost an afternoon, what the team decided and why. That knowledge dies
with the session. cairn gives agents a shared space to *propose* what they learned,
and gives you a review step before anything becomes something other agents read.

This repository holds the compiled binaries, their checksums and the installer.
The source is currently private.

## Requirements

- **A cairn account.** cairn stores knowledge in a hosted space, so it needs an
  account and a network connection. Sign-in is through GitHub.
- **An agent that speaks MCP.** Claude Code and Cursor are detected automatically.
- macOS or Linux — `darwin-arm64`, `darwin-x64`, `linux-x64`.

## Install

```sh
curl -fsSL https://raw.githubusercontent.com/Qwertic/cairn-releases/main/install.sh | sh
```

The script detects your platform, downloads the matching binary, **verifies its
SHA256 against `checksums.txt` before installing**, and places it in
`~/.local/bin` — never with `sudo`. If that directory is not on your `PATH` it
prints the exact line to add.

## Getting started

```sh
cairn login     # sign in and store a token for this machine
cairn init      # connect your agents
cairn doctor    # prove the setup actually works
```

**Run them in that order.** `cairn init` writes agent configuration, but cairn
reads and writes nothing until the machine is signed in.

`cairn doctor` is worth running even when nothing looks wrong. It spawns the exact
command your agent is configured to run and completes a real MCP handshake, which
is the only check that proves the setup works where it actually runs rather than
where you tested it.

## The loop

Once connected, your agents call cairn's MCP tools on their own: reading the team's
approved knowledge at the start of a session, and proposing what they learn as they
go. Proposals do not become team knowledge until you say so.

```sh
cairn review    # walk the pending queue: approve, reject, or edit then approve
cairn search    # find what the team knows
cairn show <id> # one claim in full
cairn status    # the queue, the approve rate, what agents have been reading
```

`cairn status` is the honest one. If claims pile up and nobody resolves them, that
shows up as a pending age that climbs.

## Commands

| | |
|---|---|
| `cairn login` / `logout` | sign in, or revoke this machine's token |
| `cairn init [--global\|--project]` | connect your agents |
| `cairn doctor [--json]` | check the setup end to end |
| `cairn review [--json]` | walk the pending queue |
| `cairn search <text> [--all]` | find what the team knows |
| `cairn show <id>` | one claim in full |
| `cairn status` / `cairn stats` | the numbers |
| `cairn import <dir>` | seed a space from a directory of markdown |
| `cairn abstracts` / `cairn curate` | write, then revise, the summaries agents navigate by |
| `cairn model` | choose the model that writes summaries |
| `cairn mcp` | the MCP server your agent spawns — you do not run this by hand |

Every command takes `--help`.

## Troubleshooting

**Start with `cairn doctor`.** It names the problem and the command that fixes it.

| what you see | what it means |
|---|---|
| `not signed in` | run `cairn login`. Since v0.3.0 an absent account is an error, not a fallback |
| `No supported agent found` | install Claude Code or Cursor, or run `cairn init --project` to write a project config anyway |
| `cairn: command not found` | `~/.local/bin` is not on your `PATH` — the installer prints the line to add |
| doctor warns about a source checkout | only relevant if you run cairn from source; a long-lived MCP server can drift from the code it was built from |
| the agent does not see the tools | restart the agent after `cairn init` — MCP servers are spawned at startup |

## Pinning a version

```sh
CAIRN_VERSION=v0.3.4 sh install.sh
```

`CAIRN_INSTALL_DIR` overrides where the binary lands.

## Reporting a problem

Open an issue on this repository. Include the output of `cairn doctor --json` and
the version from `cairn --version` — between them they identify the build and the
part of the setup that is wrong.

## Publishing

Releases here are published automatically by the `release` workflow in the private
source repository when a `v*` tag is pushed. Assets are never uploaded by hand.

## Licence

Apache-2.0. See [LICENSE](LICENSE).
