# Operations and troubleshooting

Run Compose commands from the project directory (original host: `/opt/media-server`).

```bash
cd /opt/media-server
docker compose ps
docker compose logs --tail=100 plex
docker compose logs --tail=100 radarr
docker compose logs --tail=100 sonarr
docker compose logs --tail=100 sabnzbd
```

## Plex does not start after reboot

The verified Compose file uses `restart: "no"` for Plex. Start it manually with `docker compose up -d plex`, or deliberately revise the restart policy after considering your storage mount timing.

## Missing libraries or empty folders

Check the exact host paths and whether remote disks/shares are mounted. The original library paths contain nested `Media/Movies` and `Media/TV Shows` segments. Inspect `docker compose config` for resolved mounts. Avoid creating replacement empty directories when the real storage is offline.

## Download import errors

Confirm SABnzbd writes inside `/downloads`, that Radarr/Sonarr can see the completed files there, and that UID/GID 1000 have appropriate access. Review the application logs and category settings. Do not assume hardlinks are supported across separate filesystems.

## Port conflicts

Check whether ports `7878`, `8989`, and `8080` are already occupied. Plex uses host networking and its own service ports. Adjust only after identifying the conflict.

## Backups

Back up the **application configuration directories** (`plex/`, `radarr/`, `sonarr/`, `sabnzbd/`) and document storage mounts separately. The `.gitignore` deliberately excludes these directories because they may contain credentials, databases, and other private data. Stop or quiesce services when necessary to obtain consistent database backups.

## Updates

Image tags are `latest`, so upgrades can introduce changes. Back up configurations and review upstream release notes before `docker compose pull && docker compose up -d`. Pin tested image versions if repeatable deployments are important.
