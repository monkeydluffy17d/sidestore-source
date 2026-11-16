# Luffy's App Store

A personal [SideStore](https://sidestore.io) / AltStore source. It publishes an `apps.json` that SideStore reads to show and update the apps listed below. A GitHub Action rebuilds the file every day and whenever the upstream release changes.

## Add this source to SideStore

1. Open SideStore and go to **Sources**.
2. Tap **+** and paste this URL:

   ```
   https://raw.githubusercontent.com/monkeydluffy17d/sidestore-source/refs/heads/main/apps.json
   ```

3. Confirm, then browse or update apps from the source.

## Apps included

| App | Bundle ID | Upstream |
| --- | --- | --- |
| YTKACE (YouTube tweak build) | `com.google.ios.youtube` | [itzzace/ytkace](https://github.com/itzzace/ytkace/releases) |
| EeveeSpotifyReincarnated (unpatched) | `com.spotify.client` | [SideloadLabs/EeveeSpotifyReincarnated](https://github.com/SideloadLabs/EeveeSpotifyReincarnated/releases) |

### YTKACE

- The latest YTKACE release is fetched automatically.
- If the release has several IPAs for different YouTube versions, the highest YouTube version is picked.
- The IPA is downloaded, every `.appex` extension bundle is removed, and the result is repackaged. This keeps free Apple Developer accounts from using up App IDs on extensions.
- The stripped IPA is uploaded to a GitHub Release tagged `stripped-<YTKACE tag>`. Only the two most recent stripped releases are kept.

### EeveeSpotify

- The entry is taken from the SideloadLabs AltSource, so its version, size, and download link come from upstream.
- The subtitle, description link, and screenshots are overridden here.

## How updates work

The workflow in `.github/workflows/update-apps-json.yml` runs:

- every day at 6:30 PM UTC (midnight IST),
- when a release is published in this repo,
- on manual trigger (**Actions → Update SideStore Repo → Run workflow**),
- when the workflow file itself changes.

Each run:

1. Fetches the latest YTKACE and EeveeSpotify metadata.
2. Downloads, strips, and re-uploads the YTKACE IPA.
3. Regenerates `apps.json`, validates it with `jq`, and commits it to `main` with `[skip ci]`.

## Setup (for a fork)

1. Fork or copy this repo.
2. Go to **Settings → Actions → General → Workflow permissions** and select **Read and write permissions**.
3. Update `monkeydluffy17d` in `apps.json`'s `sourceURL`, `iconURL`, and the workflow's release URLs to your own username.
4. Run the workflow once manually to generate the first `apps.json`.

No secrets are needed. The built-in `GITHUB_TOKEN` is enough.

## Notes

- Stripped IPAs are hosted in this repo's GitHub Releases. Only the latest versions are kept, so older download links stop working when they are removed.
- The stripped IPA is re-signed by SideStore when installed. Installing it may still fail if the signing certificate has been revoked.
- Use this source at your own risk. YouTube, Spotify, and the tweaks belong to their respective owners. This repo does not contain any original app code, only build tooling and metadata.

## Credits

- [itzzace](https://github.com/itzzace) for YTKACE
- [SideloadLabs](https://github.com/SideloadLabs) for the EeveeSpotify AltSource
