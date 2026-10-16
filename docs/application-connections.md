# Connect the applications

Complete each application's first-run wizard before integrating them. The exact screens can vary by release.

## SABnzbd

Open `http://SERVER_IP:8080`, configure your own authorized Usenet provider and desired download categories, and verify that completed downloads reside under the mapped `/downloads` tree. Keep the SABnzbd API key private.

## Radarr and Sonarr

Open Radarr at `http://SERVER_IP:7878` and Sonarr at `http://SERVER_IP:8989`.

- Configure SABnzbd as a download client using a reachable host address, port `8080`, and its API key. In this Compose file the applications use Docker's default project network, so `sabnzbd` is generally the appropriate hostname from Radarr/Sonarr containers.
- Choose matching categories in SABnzbd and in the corresponding app if you use categories.
- Set Radarr's root folder to `/movies` and Sonarr's to `/tv`.
- Ensure completed download paths seen by Radarr/Sonarr agree with `/downloads`. Do not add remote path mappings unless the apps actually report mismatched paths.
- Configure indexers and quality profiles according to your own services and preferences.

## Plex

Plex uses `network_mode: host` in this configuration. Open `http://SERVER_IP:32400/web` (subject to Plex's current setup and access settings). Add a Movies library rooted at `/movies` and a TV library rooted at `/tv`.

Plex sees both libraries read-only by design. Radarr and Sonarr manage media files through their writable mounts.

## Portainer

Portainer exists on the original DragonCorp host, but **no Portainer service is present in the verified Compose file**. Do not assume a Portainer port, volume name, or installation method from this repository. Document its existing deployment separately if you want it reproducible.
