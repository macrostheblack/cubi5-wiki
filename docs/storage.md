# Storage

## Drives

| Drive | Name | Type | Size | Label | Purpose |
|-------|------|------|------|-------|---------|
| Internal | — | NVMe M.2 SSD | TBC | C:\ | OS + all application installs |
| External HDD 1 | Pug | HDD | 1.81TB | D:\ | Movies + TV Shows (~1.3TB used) |
| External HDD 2 | Miranda | HDD | 1.81TB | E:\ | Music + Downloads staging |

## Folder Structure

```
D:\  (Pug)
├── Movies\        ← all movies, each in its own subfolder
└── TV Shows\      ← all TV shows, each in its own subfolder

E:\  (Miranda)
├── Music\
└── Downloads\
    ├── Complete\
    │   ├── Movies\
    │   ├── TV\
    │   └── Music\
    └── Incomplete\
```

## Notes

- qBittorrent downloads to `E:\Downloads\Incomplete` while active, moves to `E:\Downloads\Complete\[category]` when done
- Radarr moves movies from `E:\Downloads\Complete\Movies` → `D:\Movies`
- Sonarr moves TV from `E:\Downloads\Complete\TV` → `D:\TV Shows`
- Lidarr moves music from `E:\Downloads\Complete\Music` → `E:\Music` (hardlink capable — same drive)
- Movies and TV **cannot use hardlinks** — they cross drives (Miranda → Pug). Files are copied then deleted from downloads
- Music **can use hardlinks** — downloads and library are both on Miranda
- CrystalDiskInfo monitors all drive health — check periodically, especially on Pug and Miranda
- Existing media on Pug was cleaned up on 2026-03-21 — see Setup History

## Folder Naming Notes

- Sonarr root folder is `D:\TV Shows` (not `D:\TV`)
- All folder names use capitalised first letters
- Movie and TV folders were cleaned up via PowerShell scripts on 2026-03-21
- Cleanup scripts saved at `C:\Users\sorce\Desktop\Claude_Brain\`
