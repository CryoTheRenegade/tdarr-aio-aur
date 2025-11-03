# Maintainer: Jacob Ledbetter <jledbetter460@gmail.com>
pkgname=tdarr-aio
pkgver=2.49.01
pkgrel=1
pkgdesc="Transcoding application manager for processing media libraries. Server + Node with all-in-one AUR dependencies"
arch=('x86_64')
url="https://tdarr.io/"
license=('GPL3')
depends=(
  'ffmpeg-all'         # AUR package
  'handbrakecli'       # AUR package (may be 'handbrake-cli')
  'libdovi'            # AUR package
  'mkvpropedit'        # May need to use 'mkvtoolnix-cli' instead
  'mediainfo'          # Official repo or AUR
)
makedepends=('unzip')
source=(
  "https://f000.backblazeb2.com/file/tdarrs/versions/${pkgver}/linux_x64/Tdarr_Server.zip"
  "https://f000.backblazeb2.com/file/tdarrs/versions/${pkgver}/linux_x64/Tdarr_Node.zip"
  'tdarr-server.service'
  'tdarr-node.service'
  'tdarr.sysusers'
  'tdarr.tmpfiles'
)
sha256sums=(
  '1dd707d4d5e3e7f5169ff1588254fe36f71f918d3d8feedaab5843e0d4aa2238'  # Tdarr_Server.zip
  '3c714f1f81332e8df3f7d2cda2633cfdec74e0cc51847cd8599ed4d0598eea75'  # Tdarr_Node.zip
  '1dd707d4d5e3e7f5169ff1588254fe36f71f918d3d8feedaab5843e0d4aa2238'  # Tdarr_Server.zip
  '3c714f1f81332e8df3f7d2cda2633cfdec74e0cc51847cd8599ed4d0598eea75'  # Tdarr_Node.zip
  'SKIP'
  'SKIP'
)

package() {
  # Install server files
  install -dm755 "${pkgdir}/opt/tdarr/server"
  unzip -q Tdarr_Server.zip -d "${pkgdir}/opt/tdarr/server"
  chmod +x "${pkgdir}/opt/tdarr/server/Tdarr_Server"

  # Install node files
  install -dm755 "${pkgdir}/opt/tdarr/node"
  unzip -q Tdarr_Node.zip -d "${pkgdir}/opt/tdarr/node"
  chmod +x "${pkgdir}/opt/tdarr/node/Tdarr_Node"

  # Create symlinks for binaries
  install -dm755 "${pkgdir}/usr/bin"
  ln -s /opt/tdarr/server/Tdarr_Server "${pkgdir}/usr/bin/tdarr-server"
  ln -s /opt/tdarr/node/Tdarr_Node "${pkgdir}/usr/bin/tdarr-node"

  # Install systemd service files
  install -Dm644 tdarr-server.service "${pkgdir}/usr/lib/systemd/system/tdarr-server.service"
  install -Dm644 tdarr-node.service "${pkgdir}/usr/lib/systemd/system/tdarr-node.service"

  # Install sysusers and tmpfiles
  install -Dm644 tdarr.sysusers "${pkgdir}/usr/lib/sysusers.d/tdarr.conf"
  install -Dm644 tdarr.tmpfiles "${pkgdir}/usr/lib/tmpfiles.d/tdarr.conf"

  # Create config directories with proper permissions
  install -dm755 "${pkgdir}/etc/tdarr"
  install -dm755 "${pkgdir}/var/lib/tdarr"
}

