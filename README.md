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
.\publish-lan-updates.bat -Include resource/path/changed-file.hpk
```

The publisher packages through `TantraPatchTool.exe`, mirrors the LAN feed, then
validates/commits/pushes only this encrypted repository. An unchanged payload and
notice keep the current release/version. Existing release notes are preserved
unless `-Notice` supplies new text. `-NoPush` leaves a locally published draft;
build wrappers also accept `-NoPublishPatch` for explicitly local work.

The GUI `tools/SanketaPatchBuilder/SanketaPatchBuilder.exe` in the main workspace
can also create the package and LAN mirror. Run the publication command afterward
to archive/push that prepared feed without repackaging unchanged payloads.

## Feed files

- `objects/<ciphertext SHA-256>`: encrypted/compressed objects; no raw EXEs.
- `Patcher.dat`: signed release descriptor committed last during packaging.
- `UpdateList.dat`: paths, installed-file hashes and encrypted-object descriptors.
- `NotifyMsg.dat`: release notices bound by the signed descriptor.
- `version.dat`, `Tantra.dat`: signed descriptor aliases.
- `releases/<version>/`: immutable signed-release metadata inputs.
- `bootstrap/Patcher.dat`: signed client host configuration; copy this into a
  compatible client root once when selecting the feed. It differs from the feed's
  root `Patcher.dat` descriptor. Older clients also need the current `Tantra.exe`.

The active player URL remains `http://172.16.90.12/sanketa-patch`, served by XAMPP
at `C:/xampp/htdocs/sanketa-patch`. GitHub archival does not switch clients away
from this LAN host. Public GitHub downloads require a publicly accessible repo
and an explicit client host-configuration change; no GitHub token is embedded.

Private keys, source, databases, preferences and player state stay outside this
repository. The publisher rejects unexpected files and unencrypted objects.
**Older Git history contains plaintext payloads from before encryption.** Current
encrypted commits do not erase those historical files; history was not rewritten.
