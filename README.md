### Vaultwarden


## Usage

> [!IMPORTANT]
> Most modern web browsers disallow the use of Web Crypto APIs in insecure contexts. In this case, you might get an error like `Cannot read property 'importKey'`. To solve this problem, you need to access the web vault via HTTPS or localhost.
>
>This can be configured in [Vaultwarden directly](https://github.com/dani-garcia/vaultwarden/wiki/Enabling-HTTPS) or using a third-party reverse proxy ([some examples](https://github.com/dani-garcia/vaultwarden/wiki/Proxy-examples)).
>
>If you have an available domain name, you can get HTTPS certificates with [Let's Encrypt](https://letsencrypt.org/), or you can generate self-signed certificates with utilities like [mkcert](https://github.com/FiloSottile/mkcert). Some proxies automatically do this step, like Caddy or Traefik (see examples linked above).

> [!TIP]
>**For more detailed examples on how to install, use and configure Vaultwarden you can check our [Wiki](https://github.com/dani-garcia/vaultwarden/wiki).**

The main way to use Vaultwarden is via our container images which are published to [ghcr.io](https://github.com/dani-garcia/vaultwarden/pkgs/container/vaultwarden), [docker.io](https://hub.docker.com/r/vaultwarden/server) and [quay.io](https://quay.io/repository/vaultwarden/server).

There are also [community driven packages](https://github.com/dani-garcia/vaultwarden/wiki/Third-party-packages) which can be used, but those might be lagging behind the latest version or might deviate in the way Vaultwarden is configured, as described in our [Wiki](https://github.com/dani-garcia/vaultwarden/wiki).


### Docker/Podman CLI

Pull the container image and mount a volume from the host for persistent storage.<br>
You can replace `docker` with `podman` if you prefer to use podman.

```shell
docker pull vaultwarden/server:latest
docker run --detach --name vaultwarden \
  --env DOMAIN="https://vw.domain.tld" \
  --volume /vw-data/:/data/ \
  --restart unless-stopped \
  --publish 80:80 \
  vaultwarden/server:latest
```

This will preserve any persistent data under `/vw-data/`, you can adapt the path to whatever suits you.

### Docker Compose

To use Docker compose you need to create a `compose.yaml` which will hold the configuration to run the Vaultwarden container.

```yaml
services:
  app:
    image: vaultwarden/server:latest
    volumes:
      - vaultwarden_data:/data
    restart: always
    environment:
      - USER_UID=1000
      - USER_GID=1000

      - DOMAIN=http://vaultwarden.local.com
      - LOGIN_RATELIMIT_MAX_BURST=10
      - LOGIN_RATELIMIT_SECONDS=60
      - ADMIN_RATELIMIT_MAX_BURST=10
      - ADMIN_RATELIMIT_SECONDS=60
      - ADMIN_TOKEN=P@ssw0rd
      - SENDS_ALLOWED=true
      - EMERGENCY_ACCESS_ALLOWED=true
      - WEB_VAULT_ENABLED=true

      - SIGNUPS_ALLOWED=false
      - SIGNUPS_VERIFY=true
      - SIGNUPS_VERIFY_RESEND_TIME=3600
      - SIGNUPS_VERIFY_RESEND_LIMIT=5
      - SIGNUPS_DOMAINS_WHITELIST=egidegypt.com

      - SMTP_HOST=SMTP@local.com
      - SMTP_FROM=vaultwarden@local.com
      - SMTP_FROM_NAME=Vaultwarden
      - SMTP_SECURITY=off
      - SMTP_PORT=25
      # - SMTP_USERNAME=vaultwarden@local.com
      # - SMTP_PASSWORD=
      # - SMTP_AUTH_MECHANISM="Mechanism"

    ports:
      - 80:80
    networks:
      - vaultwarden-network


  backup:
      image: bruceforce/bw_backup:latest
      restart: on-failure
      depends_on:
        - app
      volumes:
        - vaultwarden_data:/data
        - vaultwarden_backup:/backup
        - '/etc/timezone:/etc/timezone:ro'
        - '/etc/localtime:/etc/localtime:ro'

      environment:
        - DB_FILE=/data/db.sqlite3
        - BACKUP_FILE=/backup/backup.sqlite3
        - BACKUP_FILE_PERMISSIONS=700
        - CRON_TIME=0 1 * * *
        - DELETE_AFTER = 48
        - TIMESTAMP=true
        - UID=0
        - GID=0

      networks:
        - vaultwarden-network
volumes:
  vaultwarden_data:
  vaultwarden_backup:

networks:
  vaultwarden-network:
    driver: bridge


```
