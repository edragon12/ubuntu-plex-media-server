# Security and GitHub publishing checklist

- Review every file before publishing to a **public** GitHub repository.
- Do not commit application configuration folders, API keys, passwords, credentials, certificates, `.env`, database files, SSH keys, or download/media contents.
- This repository contains only the verified Compose configuration and documentation, not private service settings.
- Docker daemon access and membership in the `docker` group effectively provide root-level host control; don't give AI agents unrestricted Docker socket access.
- Keep management UIs and download clients restricted to your trusted network or a properly secured remote-access solution. Do not blindly forward ports to the internet.
- `PUID=1000`, `PGID=1000`, and host paths are deployment-specific. Verify them on any replacement server.
- `compose.yaml` uses mutable `latest` tags; review updates before deployment.
- Portainer's configuration was not supplied, so it is intentionally excluded rather than guessed.

## Suggested publication workflow

```bash
git init
git add README.md compose.yaml docs/ .gitignore .env.example LICENSE
git status
git commit -m "Document DragonCorp Ubuntu media server"
```

Create an empty repository on GitHub, then follow the remote-add and push commands GitHub displays for that repository. Before the first push, review `git diff --cached` for anything sensitive.
