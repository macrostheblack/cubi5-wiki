# Media Server & Arr Stack

The core of the home server — automated media management and streaming.

## How It All Fits Together

```
Prowlarr  →  (indexers)
    ↓
Radarr / Sonarr / Lidarr  →  qBittorrent (via Surfshark VPN)
    ↓
Plex  →  streams to all devices
    ↓
Bazarr  →  automatic subtitles
```

## Applications

### Plex Media Server
| | |
|---|---|
| **WinGet ID** | `Plex.PlexMediaServer` |
| **Web UI** | `http://localhost:32400/web` |
| **Download** | [plex.tv](https://www.plex.tv/media-server-downloads/) |
| **Guide** | [Plex Support](https://support.plex.tv/) · [TRaSH Plex Guide](https://trash-guides.info/Plex/) |
| **Use Case** | Core media server. Streams movies, TV, and music to any device on the network or remotely |

### Radarr
| | |
|---|---|
| **WinGet ID** | `TeamRadarr.Radarr` |
| **Web UI** | `http://localhost:7878` |
| **Download** | [radarr.video](https://radarr.video/) |
| **Guide** | [Radarr Wiki](https://wiki.servarr.com/radarr) · [TRaSH Radarr Guide](https://trash-guides.info/Radarr/) |
| **Use Case** | Automated movie collection manager. Monitors, grabs, and organises movies via qBittorrent |

### Sonarr
| | |
|---|---|
| **WinGet ID** | `TeamSonarr.Sonarr` |
| **Web UI** | `http://localhost:8989` |
| **Download** | [sonarr.tv](https://sonarr.tv/) |
| **Guide** | [Sonarr Wiki](https://wiki.servarr.com/sonarr) · [TRaSH Sonarr Guide](https://trash-guides.info/Sonarr/) |
| **Use Case** | Same as Radarr but for TV shows. Handles episodes, seasons, and quality upgrades automatically |

### Lidarr
| | |
|---|---|
| **WinGet ID** | `TeamLidarr.Lidarr` |
| **Web UI** | `http://localhost:8686` |
| **Download** | [lidarr.audio](https://lidarr.audio/) |
| **Guide** | [Lidarr Wiki](https://wiki.servarr.com/lidarr) · [TRaSH Lidarr Guide](https://trash-guides.info/Lidarr/) |
| **Use Case** | Music collection manager. Same arr stack approach applied to artists and albums |

### Bazarr
| | |
|---|---|
| **WinGet ID** | `Morpheus.Bazarr` |
| **Web UI** | `http://localhost:6767` |
| **Download** | [bazarr.media](https://www.bazarr.media/) |
| **Guide** | [Bazarr Wiki](https://wiki.bazarr.media/) · [TRaSH Bazarr Guide](https://trash-guides.info/Bazarr/) |
| **Use Case** | Automatic subtitle downloader. Integrates with Radarr and Sonarr |

### Prowlarr
| | |
|---|---|
| **WinGet ID** | `TeamProwlarr.Prowlarr` |
| **Web UI** | `http://localhost:9696` |
| **Download** | [prowlarr.com](https://prowlarr.com/) |
| **Guide** | [Prowlarr Wiki](https://wiki.servarr.com/prowlarr) · [TRaSH Prowlarr Guide](https://trash-guides.info/Prowlarr/) |
| **Use Case** | Indexer manager. Connects torrent indexers to all arr apps from one place |

### qBittorrent
| | |
|---|---|
| **WinGet ID** | `qBittorrent.qBittorrent` |
| **Web UI** | `http://localhost:8080` |
| **Download** | [qbittorrent.org](https://www.qbittorrent.org/) |
| **Guide** | [TRaSH qBittorrent Guide](https://trash-guides.info/Downloaders/qBittorrent/Basic-Setup/) |
| **Use Case** | Download client. Receives requests from Radarr/Sonarr/Lidarr |
| **⚠️ Important** | Must be bound to Surfshark VPN interface. If VPN drops, downloads must stop — not fall back to real IP |
