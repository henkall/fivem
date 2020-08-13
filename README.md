[hub]: https://hub.docker.com/r/henkallsn/fivem_esx_bundle
[git]: https://github.com/Andruida/fivem

# [henkallsn/fivem_esx_bundle][hub] <img align="right" height="250px" src="https://portforward.com/fivem/fivem-logo.png">

This docker image allows you to run a server for FiveM, a modded GTA multiplayer program.
This image includes [txAdmin](https://github.com/tabarra/txAdmin), an in-browser server management software.
Upon first run, the configuration is generated in the host mount for the `/config` directory, and for the `/txData` directory (that contains the txAdmin configuration).
This bundle is made to run with a Mariadb server.

[![](https://images.microbadger.com/badges/version/henkallsn/fivem_esx_bundle.svg)](https://microbadger.com/images/henkallsn/fivem_esx_bundle "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/henkallsn/fivem_esx_bundle.svg)](https://microbadger.com/images/henkallsn/fivem_esx_bundle "Get your own image badge on microbadger.com")
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=RVCZJCWKD9CSW&currency_code=EUR&source=url)

## Licence Key

A freely obtained licence key is required to use this server, which should be declared as `FIVEM_LICENCE_KEY`. A tutorial on how to obtain a licence key can be found [here](https://forum.fivem.net/t/explained-how-to-make-add-a-server-key/56120)

## Usage

Use the docker-compose script as provided:

```sh
---
version: '2'
services:
# -------------------------------------------------------------------
  fivem:
    image: henkallsn/fivem_esx_bundle:latest
    stdin_open: true
    tty: true
    volumes:
      # Remember to change.
      - "/path/to/resources/folder:/config"
      # Remember to change.
      - "/path/to/txAdmin/config:/txData"
      # Remember to change.
      - "/path/to/mysql/data:/var/lib/mysql"
    ports:
      - "30120:30120"
      - "30120:30120/udp"
      - "40120:40120"
    environment:
      SERVER_PROFILE: "default"
      FIVEM_PORT: 30120
      WEB_PORT: 40120
      HOST_UID: 1000
      HOST_GID: 100
      # Remember to change.
      FIVEM_HOSTNAME: hostname-to-fivem-server
      # Remember to change.
      FIVEM_LICENCE_KEY: license-key-here
      # Remember to change.
      STEAM_WEBAPIKEY: api-key-herer
      # Database stuff ---------------
      DATABASE_SERVICE_NAME: fivem
      MYSQL_DATABASE: FiveMESX
      MYSQL_USER: database username
      MYSQL_PASSWORD: database password
      MYSQL_RANDOM_ROOT_PASSWORD: 1
      # Change to your timezone
      TZ: Europe/Copenhagen
# -------------------------------------------------------------------
  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    ports:
      - 8100-8105:80
    environment:
      - PMA_HOST=mariadb
    depends_on:
      - mariadb
# -------------------------------------------------------------------
```

_It is important that you use `interactive` and `pseudo-tty` options otherwise the container will crash on startup_
See [issue #3](https://github.com/spritsail/fivem/issues/3)

### Environment Varibles

| ** Variable name ** | **Description** | **Value** |
|---|---|---|
| PHPMYADMIN_PORT | Port used for getting to phpMyAdmin webgui. | 8100 |
| TXADMIN_PORT | Port used for getting to txAdmin webgui. | 40120 |
| FIVEM_PORT | Port used to connect to the FiveM Server |  30120 |
| STEAM_WEBAPIKEY | This is you Steam Web api key. Will be used in the server.cfg.  |  |
| FIVEM_HOSTNAME | This will be the FiveM Server name in game. Will be used in the server.cfg.  | FiveMESX Game |
| FIVEM_LICENCE_KEY | This is you FiveM License key wich is needed to start the server. Will be used in the server.cfg.  |  |
| DATABASE_SERVICE_NAME | Has to be the same as the service name. Will be used in the server.cfg. (connection string) | fivem |
| MYSQL_DATABASE | This is what you want your database to be named. Will be used in the server.cfg. (connection string) | FiveMESXDB |
| MYSQL_USER | This is the database user name. Change to what you want. Will be used in the server.cfg. (connection string) | user |
| MYSQL_PASSWORD | This is the database password. Change to what you want. Will be used in the server.cfg. (connection string) | passsword |

- `RCON_PASSWORD` - A password to use for the RCON functionality of the fxserver. If not specified, a random 16 character password is assigned. This is only used upon creation of the default configs
- `HOST_GID` - The files that are generated by the container will be written with this group ID. You must use numeric IDs. If not specified, will use `0` (root).
- `HOST_UID` - The files that are generated by the container will be written with this user ID. You must use numeric IDs. If not specified, will use `0` (root).
- `SERVER_PROFILE` - profile name used by txAdmin. If not specified, will use `dev_server`.

## Credits 
<img align="right" height="200px" src="https://raw.githubusercontent.com/tabarra/txAdmin/master/docs/banner.png">

 - This image is based on the [yobasystems/alpine-mariadb](https://hub.docker.com/r/yobasystems/alpine-mariadb) image.
 - Thanks to **tabarra** as the creator and maintainer of the [txAdmin](https://github.com/tabarra/txAdmin) repository!
 - Thanks to [Andruida][git] that I forked this code from
