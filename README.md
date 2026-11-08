# Global Tantra development updates

This is a signed incremental update feed for Tantra.exe. Install the complete
development client once, then use Tantra.exe to fetch later updates.

Publish from the game source checkout with:

    python tools/publish-github-patches.py

The default feed includes the game executable, app-local libraries, runtime
assets and client tables/layouts. Add a changed packed/loose resource with
`--include resource/path/file.hpk`. Previously selected files remain in later
manifests so testers can catch up. Bootstrap Tantra.exe changes require a fresh
base client. Player preferences, saves and offline server data are excluded.

The publisher keeps signing keys outside this repository. Clients verify the
signed manifest, hashes and monotonic version; no GitHub token goes in the game.
GitHub's raw-content cache can delay visibility of a newly published update.
Use the normal patch host for large assets and production distribution.
