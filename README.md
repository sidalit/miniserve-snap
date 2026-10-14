# miniserve snap

[![miniserve](https://snapcraft.io/miniserve/badge.svg)](https://snapcraft.io/miniserve)

Snap packaging for [miniserve](https://github.com/svenstaro/miniserve), a small web server to share files over HTTP from the command line.

Unofficial build, not affiliated with or endorsed by the miniserve project.

## Build

```bash
snapcraft pack
```

## Install and try

<a href="https://snapcraft.io/miniserve"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install miniserve                   # or a local build: ./miniserve_0.35.0_*.snap --dangerous
sudo snap connect miniserve:removable-media   # optional: /media, /mnt
miniserve ~/Downloads                         # then open http://localhost:8080
```

## Background service

The snap includes a systemd service. It is installed **disabled**, so nothing is shared until you turn it on.

By default it listens on `127.0.0.1:8080` (this machine only) and serves `/var/snap/miniserve/common/www`. This matches upstream's systemd unit, which also only allows localhost.

```bash
sudo cp -r ~/my-files/. /var/snap/miniserve/common/www/
sudo snap start --enable miniserve    # start now and at every boot
sudo snap logs miniserve              # view logs
sudo snap stop --disable miniserve    # stop and disable
```

### Settings

| Setting | Default | Description |
|---|---|---|
| `port` | `8080` | Port to listen on |
| `interfaces` | `127.0.0.1` | Address to listen on. Use `0.0.0.0` for all networks |
| `path` | `/var/snap/miniserve/common/www` | Folder to serve |
| `auth` | not set | Login, as `user:password` or `user:sha256:<hash>` |
| `upload` | `false` | Allow uploads into the served folder |

Changes apply right away. A running service restarts by itself.

```bash
sudo snap get miniserve
sudo snap set miniserve port=9000
sudo snap unset miniserve auth
```

### Serve other folders

To serve a USB drive or anything under `/media` or `/mnt`:

```bash
sudo snap connect miniserve:removable-media
sudo snap set miniserve path=/media/$USER/my-drive
```

### Open the service to your network

```bash
sudo snap set miniserve interfaces=0.0.0.0
sudo snap set miniserve auth="user:sha256:$(printf '%s' 'my-password' | sha256sum | cut -d' ' -f1)"
```

Then open `http://<this-machine-ip>:8080` from another device. Your firewall must allow the port (for example `sudo ufw allow 8080/tcp`).

Setting a login first is strongly recommended: anyone who can reach the port can read the files. Prefer the `sha256` form so the plain password is not stored in the snap settings.

## Known limitations

### Home folders

The service cannot serve folders in `/home`. It runs as root, and strict confinement only lets a program reach the home files of the user running it. Setting `path` to a home folder makes the service fail to start with `Permission denied`.

To share a home folder, run miniserve yourself instead of using the service:

```bash
miniserve ~/share
```

Or move the files to `/var/snap/miniserve/common/www` or a removable drive.

## Update to a new upstream release

Change `version` and `source-tag` in `snap/snapcraft.yaml`, then rebuild.

## License

The packaging is MIT-licensed (see `LICENSE`). miniserve is MIT-licensed; its dependencies carry their own licenses.
