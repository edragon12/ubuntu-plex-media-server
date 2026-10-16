# Storage and permissions

## Original DragonCorp mapping

| Host | Container | Used by |
| --- | --- | --- |
| `/mnt/media/movies/Media/Movies` | `/movies` | Plex (read-only), Radarr (read/write) |
| `/mnt/media/tv/Media/TV Shows` | `/tv` | Plex (read-only), Sonarr (read/write) |
| `/srv/data/downloads` | `/downloads` | Radarr, Sonarr, SABnzbd |

All four containers specify `PUID=1000`, `PGID=1000`, and `TZ=America/Chicago`. Check the UID/GID of your own host account with `id` before reusing these values.

## Preflight checks

```bash
id
findmnt /mnt/media/movies
findmnt /mnt/media/tv
ls -ld '/mnt/media/movies/Media/Movies' '/mnt/media/tv/Media/TV Shows' /srv/data/downloads
cd /opt/media-server && docker compose config
```

If media is on a network share, ensure it is mounted **before** containers start. Do not let applications write to empty directories created when a share is absent. Plex's `create_host_path: false` prevents automatic creation of its two media source directories, but the shorter Radarr/Sonarr bind syntax may create missing host paths. Confirm mounts first.

Do not recursively `chown` or `chmod` a populated library without understanding the storage backend, ACLs, and existing access requirements.

## Import behavior

Radarr/Sonarr and SABnzbd all see the download directory as `/downloads`, which avoids mismatched container download paths. Hardlink imports require compatible source/destination filesystems and mount layouts; this Compose layout does not guarantee hardlinks across separate mounts. Standard copy/move imports may still work.
