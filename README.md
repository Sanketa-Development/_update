# Sanketa encrypted updates

This repository holds the signed update feed. Game/runtime objects use **Sanketa
Seal v2** encryption and ciphertext SHA-256 filenames, so a hosted object cannot
be opened directly as `ZFLauncher.exe`. The current `Tantra.exe` verifies the
release, downloads changed objects, authenticates/decrypts/extracts them and
verifies installed file hashes before installation. Compiled clients contain the
decryption material needed to recover their game files.

## Publish from the main workspace

Normal Release client build/stage wrappers publish automatically after success:

```powershell
.\build-sanketa.bat -Component Client
```

For a staged executable or resource-only update:

```powershell
.\publish-updates.bat -Include resource/path/changed-file.hpk
```

The publisher packages through `TantraPatchTool.exe`, then
validates/commits/pushes only this encrypted repository. An unchanged payload and
notice keep the current release/version. Existing release notes are preserved
unless `-Notice` supplies new text. `-NoPush` leaves a locally published draft;
build wrappers also accept `-NoPublishPatch` for explicitly local work.
Repository publication keeps only the latest release folder and the encrypted
objects referenced by its file list; obsolete release folders/objects are removed.

The GUI `tools/SanketaPatchBuilder/SanketaPatchBuilder.exe` in the main workspace
can also create the package and an optional LAN mirror. Run the publication command afterward
to archive/push that prepared feed without repackaging unchanged payloads.

## Feed files

- `objects/<ciphertext SHA-256>`: encrypted/compressed objects; no raw EXEs.
- `Patcher.dat`: signed release descriptor committed last during packaging.
- `UpdateList.dat`: paths, installed-file hashes and encrypted-object descriptors.
- `NotifyMsg.dat`: release notices bound by the signed descriptor.
- `version.dat`, `Tantra.dat`: signed descriptor aliases.
- `releases/<version>/`: the latest release's signed metadata (one version only).
- `bootstrap/Patcher.dat`: signed client host configuration; copy this into a
  compatible client root once when selecting the feed. It differs from the feed's
  root `Patcher.dat` descriptor. Older clients also need the current `Tantra.exe`.

The default player source is `https://raw.githubusercontent.com/Sanketa-Development/_update/main`.
Existing clients can download the signed [bootstrap/Patcher.dat](https://raw.githubusercontent.com/Sanketa-Development/_update/main/bootstrap/Patcher.dat)
and replace the client-root `Patcher.dat` once. The feed-root file is a release
descriptor, not client configuration. No GitHub token is needed or embedded.
LAN publishing remains available through `publish-lan-updates.bat` in the main
workspace; normal publication needs no XAMPP server.

Private keys, source, databases, preferences and player state stay outside this
repository. The publisher rejects unexpected files and unencrypted objects.
**Older Git history contains plaintext payloads from before encryption.** Current
encrypted commits do not erase those historical files; history was not rewritten.
