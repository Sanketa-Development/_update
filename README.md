# Sanketa updates

Independent encrypted game update feeds:

- [Online](online/): use Update.exe; Start launches Sanketa.exe.
- [Offline](offline/): use UpdateOffline.exe; Start launches SanketaOffline.exe.

Each folder contains one current release and its required encrypted objects.
Old combined-feed releases and obsolete objects are retired. The new baselines
discard their old selections. Later manifests use current staged file versions
and retain required live resources from their own edition only.
Files already matching their current hashes are not downloaded again.

Install the new executable pair and its signed patch configuration together.
Online uses Patcher.dat; offline uses PatcherOffline.dat. Signed setup files are
under each feed's bootstrap folder. Game/updater EXEs are distributed with the
client; no raw EXEs, source, keys, accounts or saves belong in this repository.

The default player sources are:

- https://raw.githubusercontent.com/Sanketa-Development/_update/main/online
- https://raw.githubusercontent.com/Sanketa-Development/_update/main/offline

Offline Start works without internet. File Check requires a connection to fetch
patches. Both games reject direct launch and require their matching updater.
