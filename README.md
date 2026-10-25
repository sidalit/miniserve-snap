<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="snap/gui/miniserve-dark.png" />
  <img src="snap/gui/miniserve.png" alt="miniserve logo" width="128" />
</picture>

# miniserve

**Share files over HTTP**

[![miniserve](https://snapcraft.io/miniserve/badge.svg)](https://snapcraft.io/miniserve)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64 and arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [miniserve](https://github.com/svenstaro/miniserve), a small web server for sharing directories from the command line.

</div>

---

## Getting Started

<a href="https://snapcraft.io/miniserve"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install miniserve
miniserve ~/Downloads
```

Open `http://localhost:8080`. For the optional background service, place files in `/var/snap/miniserve/common/www` and enable it:

```bash
sudo snap start --enable miniserve.daemon
```

The service is initially disabled and listens on localhost. Stop it with `sudo snap stop --disable miniserve.daemon`.

## Configure

These options configure the background service; changes restart it when active or enabled.

| Option | Accepted value | Default |
| --- | --- | --- |
| `port` | Integer from `1` to `65535` | `8080` |
| `interfaces` | IPv4 or IPv6 address | `127.0.0.1` |
| `path` | Accessible directory | `/var/snap/miniserve/common/www` |
| `auth` | `user:password`, `user:sha256:hash`, or `user:sha512:hash` | Unset |
| `upload` | `true` or `false` | `false` |

```bash
sudo snap set miniserve port=8081 upload=true
sudo snap get miniserve
```

Set `interfaces=0.0.0.0` to share on your network. Configure `auth` first; anyone who can reach an unauthenticated server can read its files. Then open `http://<computer-ip>:<port>`; uploads are available when enabled.

Prefer `user:sha256:hash` or `user:sha512:hash` to avoid storing a plaintext password in snap settings.

For `/media` or `/mnt`, connect `sudo snap connect miniserve:removable-media` and set `path` to the directory.

## Limitations

- The background service cannot serve user home folders. Use the foreground command for those, or place files in the service's default directory.
- Hidden files directly under your normal home directory are inaccessible.
- Remote access also requires the chosen port to be allowed by your firewall.

## Build

```bash
git clone https://github.com/sidalit/miniserve-snap.git
cd miniserve-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/miniserve-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/svenstaro/miniserve/issues).

## License

Snap packaging: [MIT](https://github.com/sidalit/miniserve-snap/blob/main/LICENSE).
