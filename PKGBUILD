# Maintainer: Moritz Sauter <moritz.sauter7+aur at gmail dot com>

pkgname=i3status-rust-full-git
shortname="${pkgname%-full-*}"
pkgver=0.33.2.r3513.g0d782b856
pkgrel=6
pkgdesc='Very resourcefriendly and feature-rich replacement for i3status to use with bar programs (like i3bar and swaybar), written in pure Rust'
arch=('x86_64')
url='https://github.com/greshake/i3status-rust'
license=('GPL-3.0-only')
depends=('libpulse' 'lm_sensors' 'libpipewire' 'notmuch')
makedepends=('git' 'rust' 'pandoc' 'clang')
optdepends=('alsa-utils: for the volume block'
            'bluez: for the bluetooth block'
            'fakeroot: for the pacman block to show pending updates'
            'kdeconnect: for the kdeconnect block'
            'pipewire: for the privacy block'
            'powerline-fonts: for all themes using the powerline arrow char'
            'pulseaudio: for the volume block'
            'speedtest-cli: for the speedtest block'
            'ttf-font-awesome: for the awesome icons'
            'upower: for the battery block')
provides=("${shortname}")
conflicts=("${shortname}")
install="${shortname}.install"
source=("${shortname}::git+$url")
options=('!lto')
sha1sums=('SKIP')

pkgver() {
  cd "${shortname}"
  echo $(grep '^version =' Cargo.toml|head -n1|cut -d\" -f2).r$(git rev-list --count HEAD).g$(git describe --always)
}

prepare() {
  cd "${shortname}"
  cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
  cd "${shortname}"
  export RUSTUP_TOOLCHAIN=stable
  export CARGO_TARGET_DIR=target
  cargo build --release --features "pulseaudio maildir pipewire notmuch"
  cargo xtask generate-manpage
}

package() {
  cd "${shortname}"
  install -Dm755 target/release/i3status-rs "$pkgdir/usr/bin/i3status-rs"
  install -Dm644 man/i3status-rs.1 -t "$pkgdir/usr/share/man/man1"

  for icon_set in files/icons/*.toml; do
    install -Dm644 "$icon_set" -t "$pkgdir/usr/share/${shortname}/icons"
  done

  for theme in files/themes/*.toml; do
    install -Dm644 "$theme" -t "$pkgdir/usr/share/${shortname}/themes"
  done

  for example_config in examples/*.toml; do
    install -Dm644 "$example_config" -t "$pkgdir/usr/share/doc/${shortname}/examples"
  done
}
