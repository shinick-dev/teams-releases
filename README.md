# teams-releases

Release build pipeline for the unified **Teams / Outlook / OneNote** terminal client.

The source lives in a private repository. This repository exists only so the
six-platform build can run on GitHub-hosted runners; nothing here is the
source of truth for the application.

## What this repository does

`Release` (`.github/workflows/release.yml`) checks out the private source at a
requested tag using the `SOURCE_REPO_TOKEN` secret, then builds:

| Target | Runner |
|---|---|
| `x86_64-pc-windows-msvc` | windows-latest |
| `aarch64-pc-windows-msvc` | windows-latest |
| `x86_64-apple-darwin` | macos-15-intel |
| `aarch64-apple-darwin` | macos-15 |
| `x86_64-unknown-linux-gnu` | ubuntu-latest |
| `aarch64-unknown-linux-gnu` | ubuntu-latest |

Each job produces a release archive and a staged npm platform package. The
`release` job collects the archives, writes `SHA256SUMS.txt`, and publishes a
GitHub release for the requested tag.

## Running it

Actions → Release → Run workflow → enter the source tag (for example `v0.3.0`).

## Required secret

| Secret | Purpose |
|---|---|
| `SOURCE_REPO_TOKEN` | Fine-grained token with **Contents: Read-only** on the private source repository |

An optional `NPM_TOKEN` enables the `publish-npm` job. Without it that job
verifies the staged packages and skips publishing.

## Notes

- The release tag is created in this repository, not in the source repository.
  The source tag is what actually gets built; the release body records it.
- Binaries are unsigned.
