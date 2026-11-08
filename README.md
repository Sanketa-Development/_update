# Sanketa development patch feed

Private development repository holding signed client update artifacts. The active player patch channel remains the **LAN HTTPS/HTTP host**, as requested; clients cannot download raw contents of this private GitHub repository without authentication. No GitHub token is embedded in the client.

From the parent workspace, run `publish-lan-updates.bat` after building/staging. Add changed native resources with `-Include resource/path/file.hpk`. The script generates this repository's signed feed and mirrors its approved payloads to the LAN web root; commit/push this repository afterward to archive the checkpoint. The default migrated feed is `http://172.16.90.12/sanketa-patch`, served from `C:/xampp/htdocs/sanketa-patch`. The previous workspace's `/patch` feed is left intact.

- Root `Patcher.dat`: signed release descriptor, not client configuration.
- `UpdateList.dat` and `NotifyMsg.dat`: release manifest and notices.
- `releases/`: immutable versioned manifests/notices.
- `objects/`: content-addressed file payloads.
- `bootstrap/Patcher.dat`: signed client configuration for the selected LAN feed. Copy this file into an existing compatible client root once to select that feed.

Keep signing keys, source code, saves, preferences, server state and credentials outside this repository. Binary/signed payloads must remain byte-exact. Bootstrap Tantra.exe replacement still needs an updated base client.

GitHub-based player patching can be enabled later only after selecting a publicly reachable feed. See the parent HANDOFF.md for current publication status.
