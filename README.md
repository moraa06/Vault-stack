VaultWarden

Usage
Important

Most modern web browsers disallow the use of Web Crypto APIs in insecure contexts. In this case, you might get an error like Cannot read property 'importKey'. To solve this problem, you need to access the web vault via HTTPS or localhost.

This can be configured in Vaultwarden directly or using a third-party reverse proxy (some examples).

If you have an available domain name, you can get HTTPS certificates with Let's Encrypt, or you can generate self-signed certificates with utilities like mkcert. Some proxies automatically do this step, like Caddy or Traefik (see examples linked above).

Tip

For more detailed examples on how to install, use and configure Vaultwarden you can check our Wiki.

The main way to use Vaultwarden is via our container images which are published to ghcr.io, docker.io and quay.io.

There are also community driven packages which can be used, but those might be lagging behind the latest version or might deviate in the way Vaultwarden is configured, as described in our Wiki.

Docker/Podman CLI
Pull the container image and mount a volume from the host for persistent storage.
You can replace docker with podman if you prefer to use podman.

docker pull vaultwarden/server:latest
docker run --detach --name vaultwarden \
  --env DOMAIN="https://vw.domain.tld" \
  --volume /vw-data/:/data/ \
  --restart unless-stopped \
  --publish 80:80 \
  vaultwarden/server:latest
This will preserve any persistent data under /vw-data/, you can adapt the path to whatever suits you.

Docker Compose
To use Docker compose you need to create a compose.yaml which will hold the configuration to run the Vaultwarden container.

services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vw.domain.tld"
    volumes:
      - ./vw-data/:/data/
    ports:
      - 80:80
