# cdn-beexy-connector

Off-gallery update channel for the **Beexy Connector** WordPress plugin. This public repo is the store; `updates.beexy.io` fronts it. Clients only pull.

## What lives here

```
connector/
  info.json                           update metadata the plugin polls
  beexy-connector-<version>.zip       one signed release package per version
  beexy-connector-<version>.zip.sig   its detached Ed25519 signature (base64)
```

The plugin polls `https://updates.beexy.io/connector/info.json` (plugin-update-checker v5), downloads the `download_url` it names, and verifies the signature before installing.

## Public-but-signed

The installer is publicly downloadable, but every release is signed with an **offline Ed25519 key**, and the plugin refuses any package whose signature does not verify or whose version is below the installed one. So this public store cannot push code to an install: a tampered or swapped file fails verification. The runtime licence still governs whether an installed copy operates.

## Signing public key

Releases are signed with the key whose public half (base64 of the raw 32-byte Ed25519 key) is:

```
SQPWYljXlpsoHCZuqX26UR54ldziDO/sgkryY0mjdqM=
```

This is the value embedded in the plugin as `Beexy_Connector_Update_Verifier::PUBLIC_KEY_B64`. The private key is held offline by Voxxy and never appears in this repo or in CI.

## Serving + release process

Documented in the Beexy Connector source repo (0019):

- `stages/03-distribution/references/host-standup-runbook.md` — standing up this channel
- `stages/03-distribution/references/packaging-release-process.md` — build, sign, publish
- `stages/03-distribution/references/info-json-and-signing-spec.md` — the metadata + signature format

`info.json` must be served so it revalidates (not immutably cached); versioned ZIPs and their `.sig` may be cached immutably.
