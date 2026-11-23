# ME Wallpapers

Lock screen and desktop wallpapers for the Mieux-Être clinics, served to the PC fleet straight from this repo.

This repo is **public on purpose**. The NinjaOne scripts download the images anonymously over HTTPS
(`raw.githubusercontent.com`), so no credential has to be stored on the PCs. It holds only clinic artwork.

## Layout

| Path | Content | Used by the scripts |
|---|---|---|
| `2026/` | 2026 wallpapers, one per clinic and service (`MED` = medical clinic, `PHYSIO` = physiotherapy), plus the portrait ME logo (`2026_2e_FOND BLEU_ME.png`) | Yes |
| `Physio/` | 2026 physiotherapy variants on the blue background | No |
| repo root | 2025 blue-background wallpapers and the CMME background | No (archive) |

## Consumers

- `Set-MEWallpapers.ps1` (repo `me-wallpapers-ninjaone`): sets the lock screen and/or desktop picture chosen in NinjaOne.
- `Set-SiteConfig.ps1` (repo `site-config-ninjaone`): applies the wallpaper of the clinic a new PC is assigned to.

Both scripts build the download URL as:

```text
https://raw.githubusercontent.com/jean-philippemercier/ME-Wallpapers/main/<path>
```

Downloaded files are cached in `C:\ProgramData\ME-Wallpapers`. If GitHub can't be reached, the last cached copy is used.

## Rules for changes

- **Never rename or move a file in `2026/`** without updating the `$Wallpapers` table in both scripts first. The scripts
  reference exact paths; a renamed file makes the PCs fall back to their cached copy, or fail on a new PC.
- Accented file names are stored decomposed (NFD): `É` is `E` + U+0301. In the scripts' URL-encoded paths it is
  written `E%CC%81`, and spaces are `%20`.
- To add a wallpaper: commit the image under `2026/`, add its line to `$Wallpapers` in both scripts, then add the
  new name to the NinjaOne dropdowns (`lockscreenWallpaper`, `desktopWallpaper`).
- PNG or JPG. Keep files under a few MB: every PC downloads its picture once and keeps it in the cache.
