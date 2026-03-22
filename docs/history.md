# Setup History

A full log of everything done to the Cubi 5, in chronological order.

---

## 2026-03-21 — Initial Setup

### OS — Tiny11 Build

- Downloaded Windows 11 ISO from Microsoft
- Built debloated Tiny11 image using [tiny11builder](https://github.com/ntdevlabs/tiny11builder) (`tiny11maker.ps1`)
  - Mounted Windows 11 ISO to drive `F:`
  - Used external HDD as scratch disk
  - Selected image index **6 (Windows 11 Pro)**
  - Script removed: Edge, OneDrive, Xbox, Clipchamp, Teams, Cortana, and other bloat
  - Disabled telemetry, Copilot, sponsored apps, BitLocker auto-encryption
  - Bypassed TPM/Secure Boot/RAM requirements via registry
- Flashed `tiny11.iso` to USB using **Rufus**
  - Rufus options selected: Remove TPM/RAM/Secure Boot requirement, Remove Microsoft account requirement, Disable BitLocker automatic encryption
- Booted Cubi 5 from USB (boot menu via `Delete` / `F11` on POST)
- **Note:** MSI Fast Boot was left disabled for USB boot to work
- Completed OOBE with local account (`sorce`) — no Microsoft account required (handled by tiny11's `autounattend.xml`)

### Drivers

Downloaded all drivers from [MSI Cubi 5 12M Support Page](https://www.msi.com/Business-Productivity-PC/Cubi-5-12M/support#driver).

Installed in this order:

| # | Driver | Version | Notes |
|---|--------|---------|-------|
| 1 | WiFi | 22.150.0.3 | ⚠️ Installed out of order by mistake — reinstalled at step 9 |
| 2 | Chipset | 10.1.19159.8331 | Rebooted after |
| 3 | ME (Intel Management Engine) | 2420.6.16.0 | Rebooted after |
| 4 | Serial IO | 30.100.2212.4 | — |
| 5 | IGD (Intel Graphics) | 31.0.101.3301 | — |
| 6 | Audio (UAD) | — | — |
| 7 | LAN | 10062_10112022 | — |
| 8 | Bluetooth | 22.150.0.6 | — |
| 9 | WiFi | 22.150.0.3 | Reinstalled to ensure clean install over correct chipset base |
| 10 | MSI Center | — | — |

Rebooted after Chipset and ME drivers. Final reboot after all drivers complete.

**Skipped:** DTT (Dynamic Tuning — not needed for always-on server), Intel GNA (AI inference — not needed)

### Windows Updates

- Ran Windows Update after driver installation
- Completed all available updates before proceeding to software installation

### Software Installation

- Installed UniGetUI via WinGet:
  ```
  winget install --exact --id Devolutions.UniGetUI --source winget
  ```
- Used UniGetUI to bulk install all applications via WinGet:
  - Plex Media Server, Radarr, Sonarr, Lidarr, Bazarr, Prowlarr, qBittorrent
  - CrystalDiskInfo, WizTree, HWiNFO64
  - Everything (voidtools), Bulk Rename Utility
  - PowerToys, AutoHotkey, Notepad++, Surfshark
  - Firefox, VLC
- Manual installs: WinRAR (licence), ProcessLasso (licence), MSI Center (from driver package), EverythingToolbar (GitHub release)
- Exported UniGetUI bundle (.ubundle) saved to USB drive for future reinstalls
- Fixed permissions on `C:\ProgramData` for arr apps (Prowlarr, Radarr, Sonarr, Lidarr, Bazarr) via PowerShell script
  - Script saved at `C:\Users\sorce\Desktop\Claude_Brain\fix-arr-permissions.ps1`

### Wiki Setup

- Created this wiki using MkDocs + Material theme
- Wiki files located at `C:\Users\sorce\Desktop\Claude_Brain\Wiki`
- To be hosted on GitHub Pages

### Folder Structure & Media Cleanup

- Created Miranda (E:\\) folder structure for downloads:
  - `E:\Music\`, `E:\Downloads\Complete\Movies\`, `E:\Downloads\Complete\TV\`, `E:\Downloads\Complete\Music\`, `E:\Downloads\Incomplete\`
- Cleaned up `D:\Movies` using PowerShell script (`cleanup-movies-run.ps1`):
  - Wrapped 22 loose .mkv files into their own subfolders
  - Renamed 2 `www.` prefixed folders
  - Deleted 5 stray `.parts` files
- Cleaned up `D:\TV Shows` using PowerShell script (`cleanup-tv-run.ps1`):
  - Wrapped 2 loose video files into their own subfolders
  - Renamed 1 `www.` prefix folder
  - Deleted 1 stray `.parts` file
- Cleanup scripts saved at `C:\Users\sorce\Desktop\Claude_Brain\`

---

## 2026-03-21 — Arr Stack Configuration

### qBittorrent
- Configured download paths: Default save → `E:\Downloads\Complete`, Incomplete → `E:\Downloads\Incomplete`
- Set saving management to `Automatic`
- Added categories: `movies` → `E:\Downloads\Complete\Movies`, `tv` → `E:\Downloads\Complete\TV`, `music` → `E:\Downloads\Complete\Music`
- Connection: TCP only, UPnP disabled, random port disabled
- BitTorrent: Allow encryption, anonymous mode disabled, seeding limits managed by arr apps
- Web UI: Bypass auth for localhost enabled, CSRF protection disabled

### Prowlarr
- Authentication set up (Forms login)
- Fixed `C:\ProgramData` permissions to allow launch
- Connected to Radarr, Sonarr, and Lidarr via API keys (Sync Level: Full Sync)
- Connected qBittorrent as download client
- Indexers: added later — see Indexers section below

### Radarr
- Authentication set up
- Media Management: Rename Movies enabled, naming scheme set per TRaSH guide
- Root folder: `D:\Movies`
- Download client: qBittorrent (category: `movies`)
- Quality profile: HD Movies (Remux-1080p, Bluray-1080p, WEB 1080p, Bluray-720p, WEB 720p)
- Connected to Prowlarr

### Sonarr
- Authentication set up
- Media Management: Rename Episodes enabled, naming scheme set per TRaSH guide
- Root folder: `D:\TV Shows`
- Download client: qBittorrent (category: `tv`)
- Quality profile: HD TV
- Connected to Prowlarr

### Lidarr
- Authentication set up
- Media Management: Rename Tracks enabled
- Root folder: `E:\Music`
- Download client: qBittorrent (category: `music`)
- Connected to Prowlarr
- Artists to be added to library separately

### Bazarr
- Authentication set up
- Connected to Sonarr (localhost:8989) and Radarr (localhost:7878) via API keys
- Language profile: English, cutoff English, minimum score 90
- Applied to both series and movies as default profile
- Providers added: OpenSubtitles.com, Subscene, Supersubtitles, TVSubtitles
- Subtitles stored alongside media files
- Automatic subtitle synchronisation enabled
- Applied English language profile to existing library (series + movies)

### Plex
- Account created/signed in
- Libraries added:
  - Movies → `D:\Movies` (agent: Plex Movie)
  - TV Shows → `D:\TV Shows` (agent: Plex TV Series, ordering: TheTVDB)
  - Music → `E:\Music`
- Key settings per TRaSH guide:
  - Hardware acceleration enabled (Intel QuickSync)
  - Video preview thumbnails disabled (saves disk/CPU)
  - Intro/credits detection enabled (scheduled task)
  - Allow media deletion disabled (Radarr/Sonarr manage files)
  - Scanner runs at lower priority
  - Database cache: 1024MB
  - Relay disabled
  - HDR tone mapping enabled, algorithm: hable
- Initial library scan completed

### Autostart Configuration

- Confirmed Windows Services (Automatic) for: Bazarr, Lidarr, Prowlarr, Radarr
- Sonarr was running as standalone process conflicting with service — killed process, registered as Windows Service via `ServiceInstall.exe`, confirmed running after reboot
- Plex — Launch at Login enabled via system tray settings
- qBittorrent — shortcut added to `shell:startup`, configured to run minimised
- All services confirmed running after clean reboot

### VPN Binding

- qBittorrent bound to **SurfsharkWireGuard** network interface
  - Tools → Options → Advanced → Network Interface → SurfsharkWireGuard
  - Surfshark protocol locked to WireGuard in Surfshark app settings
  - Kill switch confirmed: qBittorrent loses connectivity when Surfshark disconnects

### Indexers

- Added **TorrentLeech** to Prowlarr as primary indexer
  - Categories: Movies, Movies HD, Movies SD, TV, TV HD, TV SD
  - Minimum seeders: 1
  - Sync profile: Standard
  - Synced automatically to Radarr and Sonarr
- Decision: TorrentLeech only for now, no public trackers — safer approach with VPN

### tinyMediaManager

- Installed tinyMediaManager v5
- Used to clean up and match existing movie and TV library before Radarr/Sonarr import
- Scrapers: TMDb for movies, TheTVDB for TV shows
- Ran on `D:\Movies` and `D:\TV Shows`

### Library Import

- Radarr: existing movies imported from `D:\Movies` via Manual Import — Radarr now has ownership
- Sonarr: existing TV shows imported from `D:\TV Shows` via Manual Import — Sonarr now has ownership
- Plex account corrected from socerorsisle@gmail.com to adamshiggs97@gmail.com
- Plex settings verified per TRaSH guide:
  - Client Network: IPv4 only
  - Certification Country: Australia
  - Cinema Trailers: personal preference
  - Prefer local metadata: disabled
  - Local posters: enabled
