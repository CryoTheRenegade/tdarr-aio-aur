# tdarr-aio-aur
A arch PKGBuild for making a ready to go TDARR server with libdovi and latest FFMPEG, HandbrakeCLI, mkvpropedit, and mediainfo all via AUR. Hopefully will auto-update when i can figure out GH actions.

## Dependencies

This package depends on the following AUR packages:
- `ffmpeg-all` - Full-featured FFmpeg with all codecs
- `handbrakecli` - HandBrake command-line interface
- `libdovi` - Dolby Vision libraries
- `mkvpropedit` - MKV property editor (from mkvtoolnix-cli package)
- `mediainfo` - Media information library

## Building

1. Install base-devel and git if you haven't already:
   ```bash
   sudo pacman -S --needed base-devel git
   ```

2. Install AUR dependencies (using yay or paru):
   ```bash
   paru ffmpeg-all handbrakecli libdovi mkvpropedit mediainfo
   ```
   
   Or manually with makepkg for each AUR package.

3. Build and install the package:
   ```bash
   makepkg -si
   ```

4. Enable and start services:
   ```bash
   sudo systemctl enable --now tdarr-server
   sudo systemctl enable --now tdarr-node
   ```

## Configuration

TDARR configuration files are located in:
- Server: `/opt/tdarr/server/configs/`
- Node: `/opt/tdarr/node/configs/`

The services run as the `tdarr` user with data stored in `/var/lib/tdarr/`.

## Updating the Version

To update to a new TDARR version:
1. Update `pkgver` in PKGBUILD
2. Download the new ZIP files and update `sha256sums`:
   ```bash
   sha256sum Tdarr_Server.zip Tdarr_Node.zip
   ```
3. Rebuild the package

## GitHub Actions

This repository includes automated GitHub Actions workflows:

### Daily Hash Updates
- **Workflow**: `.github/workflows/update-hashes.yml`
- **Schedule**: Runs daily at 00:00 UTC
- **Function**: Automatically downloads TDARR Server and Node binaries, calculates SHA256 hashes, and updates the PKGBUILD file
- **Manual Trigger**: Available via `workflow_dispatch` in GitHub Actions UI

### Version Checking
- **Workflow**: `.github/workflows/check-version.yml`
- **Schedule**: Runs daily at 01:00 UTC
- **Function**: Checks for new TDARR versions and creates GitHub issues when updates are available

Both workflows will automatically commit changes to the repository when updates are found.

## Notes

- SHA256 hashes are automatically updated daily by GitHub Actions
- If `mkvpropedit` is not available as a separate package, you may need to use `mkvtoolnix-cli` instead
- The package creates a `tdarr` system user automatically via sysusers.d