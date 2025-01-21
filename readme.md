# Ghost Compose Example
Docker compose for easily spinning up a copy of the Ghost blog using:

| Project | Version | Comment | Links |
|----|----|----|----|
| Traefik | v3.x | Reverse proxy | [Project Site](https://traefik.io/) <br> [Docker Hub](https://hub.docker.com/_/traefik) |
| Tecnativa | latest | Socket proxy (for host security) | [Project Site](https://github.com/Tecnativa/docker-socket-proxy) <br> [Docker Hub](https://hub.docker.com/r/tecnativa/docker-socket-proxy) |
| Crowdsec | latest | Agent for bouncing bad traffic based on community reputation | [Project Site](https://www.crowdsec.net/) <br> [Docker Hub](https://hub.docker.com/r/crowdsecurity/crowdsec) |
| Crowdsec Traefik<br>Bouncer Plugin | v1.3.5 | Plugin that is responsible for bouncing bad traffic | [Github](https://github.com/maxlerebourg/crowdsec-bouncer-traefik-plugin) |
| MySQL | v9.x | Database server | [Project Site](https://www.mysql.com/) <br> [Docker Hub](https://hub.docker.com/_/mysql) |
| Ghost | v5.x | Blogging platform / CMS | [Project Site](https://ghost.org/) <br> [Docker Hub](https://hub.docker.com/_/ghost) |


**Traefik** is configured out-of-the-box for SSL via **Let's Encrypt** and DNS challenges at **Cloudflare** to get wildcard certs, in case you want to spin up other services in this stack. Installation section below tells you what's needed to set this all up.



## Notes and Caveats
Generally speaking, I prefer to pin major versions in my compose files so there are no surprise upgrades with breaking changes (hopefully). I aim for minor versions so that recycling the host should pull patch updates. There are some exceptions for this (like the socket proxy, which I've never seen break).

You can either replace all image versions with `latest` tags, or keep an eye on the above links for updates and manually apply them by updating the compose file.

**Remaining TODO Items:**
- [x] Fix issue with loading CrowdSec middleware
- [ ] Figure out how to use environment variable for changing the plugin's version more easily
- [ ] Test it


# Installation
See the docs folder.


# Updating
Generally speaking, you'll want to compose down, change any version numbers you want, pull latest, and then compose up.

```bash
# change version numbers in config files, update env file, etc
docker compose down
docker compose pull
docker compose up -d
```
