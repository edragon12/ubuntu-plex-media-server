# ubuntu-plex-media-server

A documented Docker Compose setup for **Plex**, **Radarr**, **Sonarr**, and **SABnzbd** on Ubuntu. The Compose file in this repository reproduces the configuration supplied from a working DragonCorp server on October 16, 2026. **Portainer is also installed on that server but is managed separately; its deployment configuration is not included here.**

> This is a configuration snapshot and setup guide, not a full backup or one-click installer. Media, application databases, API keys, download-provider details, and credentials are deliberately excluded.

## Architecture

| Service | Image | Access | Container media/download paths |
| --- | --- | --- | --- |
| Plex | `lscr.io/linuxserver/plex:latest` | Host network; typically `http://SERVER_IP:32400/web` | `/movies` (read-only), `/tv` (read-only) |
| Radarr | `lscr.io/linuxserver/radarr:latest` | `http://SERVER_IP:7878` | `/movies`, `/downloads` |
| Sonarr | `lscr.io/linuxserver/sonarr:latest` | `http://SERVER_IP:8989` | `/tv`, `/downloads` |
| SABnzbd | `lscr.io/linuxserver/sabnzbd:latest` | `http://SERVER_IP:8080` | `/downloads` |

`SERVER_IP` is the LAN address of your own Ubuntu host. Do not expose these application interfaces directly to the public internet.

## Verified host paths on the original server

| Purpose | Host path |
| --- | --- |
| Compose project | `/opt/media-server/compose.yaml` |
| Movies | `/mnt/media/movies/Media/Movies` |
| TV shows | `/mnt/media/tv/Media/TV Shows` |
| Downloads | `/srv/data/downloads` |
| App settings | `./plex`, `./radarr`, `./sonarr`, `./sabnzbd` relative to the Compose project directory |

The nested `Media/Movies` and `Media/TV Shows` directory names are intentional. Plex bind mounts use `create_host_path: false`, so the source directories must exist before startup.

## Deploy on Ubuntu

1. Install Ubuntu Server, update the system, and install **Docker Engine** with the **Docker Compose plugin** using the [official Docker Ubuntu instructions](https://docs.docker.com/engine/install/ubuntu/). Ensure your user is authorized to manage Docker; Docker access is effectively root-level privilege.
2. Mount or connect your media storage, and verify the **actual** movie and TV directories. Adjust paths in `compose.yaml` for your machine before starting services. See [Storage and permissions](docs/storage-and-permissions.md).
3. Create a project directory, e.g. `/opt/media-server`, and place `compose.yaml` there. Keep application configuration folders outside Git.
4. Check the YAML and pull images:

   ```bash
   cd /opt/media-server
   docker compose config
   docker compose pull
   ```

5. Start the services:

   ```bash
   docker compose up -d
   docker compose ps
   ```

6. Open the application interfaces listed above and complete their first-run setup. Follow [Application connections](docs/application-connections.md).

**Important:** This snapshot intentionally retains `restart: "no"` for Plex. Plex will **not** automatically restart after a reboot or failure. If you prefer automatic startup, review that policy and change it deliberately. Radarr, Sonarr, and SABnzbd use `unless-stopped`.

## Common commands

```bash
cd /opt/media-server
# Status
docker compose ps
# Logs (omit -f to return immediately)
docker compose logs --tail=100
# Pull updated images and recreate containers (review release notes/backups first)
docker compose pull
docker compose up -d
# Stop services without deleting application configuration
docker compose down
```

## Documentation

- [Storage and permissions](docs/storage-and-permissions.md)
- [Application connections](docs/application-connections.md)
- [Operations and troubleshooting](docs/operations-and-troubleshooting.md)
- [Security and publishing](docs/security-and-publishing.md)

## What is not captured

This repository does **not** claim to reproduce your host's disk mounting configuration, hardware transcoding, firewall rules, VPN, indexers, download-provider credentials, API keys, Portainer deployment, or application-internal settings. Those require separate verification and documentation.

## License

See [LICENSE](LICENSE).
