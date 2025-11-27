# HomeLabo Minecraft

Containerized Fabric server with VanillaTweaks datapacks/resource packs and a few quality-of-life defaults. Run it locally or on a small VPS with Docker Compose.

## Quick start

```bash
cd minecraft
docker compose up -d
```

The first start pulls `itzg/minecraft-server`, accepts the EULA, downloads Fabric for the specified version, applies the VanillaTweaks packs listed in the JSON files, and creates a data directory at `/home/toix/data/config/minecraft`.

## Configuration

Configuration lives in `minecraft/docker-compose.yml`:
- `TYPE: FABRIC` with `VERSION: 1.21.10`
- VanillaTweaks lists are mounted read-only from the repo: `vanillatweaks-*.json`
- World/data persists to `/home/toix/data/config/minecraft` (change the host path if needed)
- RCON exposed on `25575` with the password set in the compose file
- Query enabled; whitelist and ops synchronize on start; custom server icon and MOTD applied
- Logs shipped via GELF to `udp://192.168.2.44:12201` with tag `minecraft`

### Tweaking VanillaTweaks

Edit the JSON manifests in `minecraft/` to add or remove datapacks/resource packs/crafting tweaks. The container syncs them on start because `REMOVE_OLD_VANILLATWEAKS` is enabled.

### Ports

- 25565/tcp → Minecraft
- 8123/tcp → Reserved for Dynmap (or similar) if you enable it later

## Maintenance

- To view logs: `docker compose logs -f` from `minecraft/`
- To update the server version, change `VERSION` in the compose file and restart
- To change secrets (RCON password, whitelist, ops), edit the environment variables before starting the container
- Back up the host data directory (`/home/toix/data/config/minecraft`) to keep worlds and configs safe

## Tips

- Consider moving secrets (RCON password) into an `.env` file referenced by `docker-compose.yml`
- If you change the host data path, copy any existing world folder into the new location before starting
