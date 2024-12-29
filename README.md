![Vaultwarden Logo](./resources/vaultwarden-logo-auto.svg)

An alternative server implementation of the Bitwarden Client API, written in Rust and compatible with [official Bitwarden clients](https://bitwarden.com/download/) [[disclaimer](#disclaimer)], perfect for self-hosted deployment where running the official resource-heavy service might not be ideal.

---

[![GitHub Release](https://img.shields.io/github/release/dani-garcia/vaultwarden.svg?style=for-the-badge&logo=vaultwarden&color=005AA4)](https://github.com/dani-garcia/vaultwarden/releases/latest)
[![ghcr.io Pulls](https://img.shields.io/badge/dynamic/json?style=for-the-badge&logo=github&logoColor=fff&color=005AA4&url=https%3A%2F%2Fipitio.github.io%2Fbackage%2Fdani-garcia%2Fvaultwarden%2Fvaultwarden.json&query=%24.downloads&label=ghcr.io%20pulls&cacheSeconds=14400)](https://github.com/dani-garcia/vaultwarden/pkgs/container/vaultwarden)
[![Docker Pulls](https://img.shields.io/docker/pulls/vaultwarden/server.svg?style=for-the-badge&logo=docker&logoColor=fff&color=005AA4&label=docker.io%20pulls)](https://hub.docker.com/r/vaultwarden/server)
[![Quay.io](https://img.shields.io/badge/quay.io-download-005AA4?style=for-the-badge&logo=redhat&cacheSeconds=14400)](https://quay.io/repository/vaultwarden/server) <br>
[![Contributors](https://img.shields.io/github/contributors-anon/dani-garcia/vaultwarden.svg?style=flat-square&logo=vaultwarden&color=005AA4)](https://github.com/dani-garcia/vaultwarden/graphs/contributors)
[![Forks](https://img.shields.io/github/forks/dani-garcia/vaultwarden.svg?style=flat-square&logo=github&logoColor=fff&color=005AA4)](https://github.com/dani-garcia/vaultwarden/network/members)
[![Stars](https://img.shields.io/github/stars/dani-garcia/vaultwarden.svg?style=flat-square&logo=github&logoColor=fff&color=005AA4)](https://github.com/dani-garcia/vaultwarden/stargazers)
[![Issues Open](https://img.shields.io/github/issues/dani-garcia/vaultwarden.svg?style=flat-square&logo=github&logoColor=fff&color=005AA4&cacheSeconds=300)](https://github.com/dani-garcia/vaultwarden/issues)
[![Issues Closed](https://img.shields.io/github/issues-closed/dani-garcia/vaultwarden.svg?style=flat-square&logo=github&logoColor=fff&color=005AA4&cacheSeconds=300)](https://github.com/dani-garcia/vaultwarden/issues?q=is%3Aissue+is%3Aclosed)
[![AGPL-3.0 Licensed](https://img.shields.io/github/license/dani-garcia/vaultwarden.svg?style=flat-square&logo=vaultwarden&color=944000&cacheSeconds=14400)](https://github.com/dani-garcia/vaultwarden/blob/main/LICENSE.txt) <br>
[![Dependency Status](https://img.shields.io/badge/dynamic/xml?url=https%3A%2F%2Fdeps.rs%2Frepo%2Fgithub%2Fdani-garcia%2Fvaultwarden%2Fstatus.svg&query=%2F*%5Blocal-name()%3D'svg'%5D%2F*%5Blocal-name()%3D'g'%5D%5B2%5D%2F*%5Blocal-name()%3D'text'%5D%5B4%5D&style=flat-square&logo=rust&label=dependencies&color=005AA4)](https://deps.rs/repo/github/dani-garcia/vaultwarden)
[![GHA Release](https://img.shields.io/github/actions/workflow/status/dani-garcia/vaultwarden/release.yml?style=flat-square&logo=github&logoColor=fff&label=Release%20Workflow)](https://github.com/dani-garcia/vaultwarden/actions/workflows/release.yml)
[![GHA Build](https://img.shields.io/github/actions/workflow/status/dani-garcia/vaultwarden/build.yml?style=flat-square&logo=github&logoColor=fff&label=Build%20Workflow)](https://github.com/dani-garcia/vaultwarden/actions/workflows/build.yml) <br>
[![Matrix Chat](https://img.shields.io/matrix/vaultwarden:matrix.org.svg?style=flat-square&logo=matrix&logoColor=fff&color=953B00&cacheSeconds=14400)](https://matrix.to/#/#vaultwarden:matrix.org)
[![GitHub Discussions](https://img.shields.io/github/discussions/dani-garcia/vaultwarden?style=flat-square&logo=github&logoColor=fff&color=953B00&cacheSeconds=300)](https://github.com/dani-garcia/vaultwarden/discussions)
[![Discourse Discussions](https://img.shields.io/discourse/topics?server=https%3A%2F%2Fvaultwarden.discourse.group%2F&style=flat-square&logo=discourse&color=953B00)](https://vaultwarden.discourse.group/)

> [!IMPORTANT]
> **When using this server, please report any bugs or suggestions directly to us (see [Get in touch](#get-in-touch)), regardless of whatever clients you are using (mobile, desktop, browser...). DO NOT use the official Bitwarden support channels.**

<br>

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
