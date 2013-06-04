# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=chromium
pkgver=27.0.1453.110
pkgrel=1
pkgdesc="The open-source project behind Google Chrome, an attempt at creating a safer, faster, and more stable browser"
arch=('i686' 'x86_64')
url="http://www.chromium.org/"
license=('BSD')
depends=('gtk2' 'nss' 'alsa-lib' 'xdg-utils' 'bzip2' 'libevent' 'libxss'
         'libgcrypt' 'ttf-font' 'udev' 'dbus' 'flac' 'opus' 'libwebp'
         'speech-dispatcher' 'pciutils' 'libpulse' 'desktop-file-utils'
         'hicolor-icon-theme')
makedepends=('python2' 'perl' 'gperf' 'yasm' 'mesa' 'libgnome-keyring'
             'elfutils' 'subversion' 'nacl-toolchain-newlib')
optdepends=('kdebase-kdialog: needed for file dialogs in KDE')
backup=('etc/chromium/default')
install=chromium.install
source=(http://commondatastorage.googleapis.com/chromium-browser-official/$pkgname-$pkgver.tar.xz
        chromium.desktop
        chromium.default
        chromium.sh
        chromium-pnacl-r0.patch
        chromium-27.0.1453.93-allow-sample-rate-pass-through-on-linux.patch
        chromium-27.0.1453.93-fix-crash-when-quickly-dragging-a-new-tab.patch
        chromium-26.0.1410.43-speechd-0.8.patch)
sha256sums=('85ebae343507bcd2393f2e159f2204e47a2f5e2a45fa9f3a389bcdfc17d4c1f8'
            '09bfac44104f4ccda4c228053f689c947b3e97da9a4ab6fa34ce061ee83d0322'
            '478340d5760a9bd6c549e19b1b5d1c5b4933ebf5f8cfb2b3e2d70d07443fe232'
            '4999fded897af692f4974f0a3e3bbb215193519918a1fa9b31ed51e74a2dccb9'
            '9875ffcc0e9ae9420876ac66b130b1b017d445a031d43cbe0119793e1fb3781c'
            'c57d760589af1566469fbba0d85624b53eb07a3077485599f599a226dbd4d2a8'
            'd530f52cb485ff8da035b38ffebe171309a1d8e515040f5e6398ecb286336797'
            '23b04468881642ffdc8457016c8f91df395dfccb4af2ad6b758168180ae070f3')

# Google API keys (see http://www.chromium.org/developers/how-tos/api-keys)
# Note: These are for Arch Linux use ONLY. For your own distribution, please
# get your own set of keys. Feel free to contact foutrelis@archlinux.org for
# more information.
_google_api_key=AIzaSyDwr302FpOSkGRpLlUpPThNTDPbXcIn_FM
_google_default_client_id=413772536636.apps.googleusercontent.com
_google_default_client_secret=0ZChLK6AxeA3Isu96MkwqDR4

prepare() {
  cd "$srcdir/$pkgname-$pkgver"

  # Fix build without pnacl (patch from Gentoo)
  patch -Np0 -i "$srcdir/chromium-pnacl-r0.patch"

  # Allow sample rate pass through on Linux
  # https://code.google.com/p/chromium/issues/detail?id=229918
  patch -Np1 -i "$srcdir/chromium-27.0.1453.93-allow-sample-rate-pass-through-on-linux.patch"

  # Fix crash when quickly dragging a new tab
  # https://code.google.com/p/chromium/issues/detail?id=228918
  patch -Np1 -i "$srcdir/chromium-27.0.1453.93-fix-crash-when-quickly-dragging-a-new-tab.patch"

  # Fix header location for speech-dispatcher 0.8 (patch from PLD Linux)
  patch -Np2 -i "$srcdir/chromium-26.0.1410.43-speechd-0.8.patch"

  # Use Python 2
  find . -type f -exec sed -i -r \
    -e 's|/usr/bin/python$|&2|g' \
    -e 's|(/usr/bin/python2)\.4$|\1|g' \
    {} +
  # There are still a lot of relative calls which need a workaround
  mkdir "$srcdir/python2-path"
  ln -s /usr/bin/python2 "$srcdir/python2-path/python"

  # Prepare NaCL toolchain
  mkdir -p out/Release/obj/gen/sdk/toolchain
  cp -a /usr/lib/nacl-toolchain-newlib \
    out/Release/obj/gen/sdk/toolchain/linux_x86_newlib
  touch out/Release/obj/gen/sdk/toolchain/linux_x86_newlib/stamp.untar
}

build() {
  cd "$srcdir/$pkgname-$pkgver"

  export PATH="$srcdir/python2-path:$PATH"

  # CFLAGS are passed through release_extra_cflags below
  export -n CFLAGS CXXFLAGS

  # Silence "typedef 'x' locally defined but not used" warnings
  CFLAGS+=' -Wno-unused-local-typedefs'

  build/gyp_chromium --depth=. \
    -Dgoogle_api_key=$_google_api_key \
    -Dgoogle_default_client_id=$_google_default_client_id \
    -Dgoogle_default_client_secret=$_google_default_client_secret \
    -Dwerror= \
    -Dlinux_link_gsettings=1 \
    -Dlinux_link_libpci=1 \
    -Dlinux_link_libspeechd=1 \
    -Dlinux_link_pulseaudio=1 \
    -Dlinux_sandbox_path=/usr/lib/chromium/chromium-sandbox \
    -Dlinux_strip_binary=1 \
    -Dlinux_use_gold_binary=0 \
    -Dlinux_use_gold_flags=0 \
    -Dlinux_use_tcmalloc=0 \
    -Drelease_extra_cflags="$CFLAGS" \
    -Dffmpeg_branding=Chrome \
    -Dproprietary_codecs=1 \
    -Duse_system_bzip2=1 \
    -Duse_system_flac=1 \
    -Duse_system_ffmpeg=0 \
    -Duse_system_libevent=1 \
    -Duse_system_libjpeg=1 \
    -Duse_system_libpng=1 \
    -Duse_system_libwebp=1 \
    -Duse_system_libxml=0 \
    -Duse_system_opus=1 \
    -Duse_system_ssl=0 \
    -Duse_system_xdg_utils=1 \
    -Duse_system_yasm=1 \
    -Duse_system_zlib=0 \
    -Duse_gconf=0 \
    -Ddisable_glibc=1 \
    -Ddisable_pnacl=1 \
    -Ddisable_newlib_untar=1 \
    -Ddisable_sse2=1

  make chrome chrome_sandbox BUILDTYPE=Release
}

package() {
  cd "$srcdir/$pkgname-$pkgver"

  install -D out/Release/chrome "$pkgdir/usr/lib/chromium/chromium"

  install -Dm4755 -o root -g root out/Release/chrome_sandbox \
    "$pkgdir/usr/lib/chromium/chromium-sandbox"

  cp out/Release/{*.pak,libffmpegsumo.so,nacl_helper{,_bootstrap}} \
    out/Release/{libppGoogleNaClPluginChrome.so,nacl_irt_*.nexe} \
    "$pkgdir/usr/lib/chromium/"

  # Allow users to override command-line options
  install -Dm644 "$srcdir/chromium.default" "$pkgdir/etc/chromium/default"

  cp -a out/Release/locales "$pkgdir/usr/lib/chromium/"

  install -Dm644 out/Release/chrome.1 "$pkgdir/usr/share/man/man1/chromium.1"

  install -Dm644 "$srcdir/chromium.desktop" \
    "$pkgdir/usr/share/applications/chromium.desktop"

  for size in 22 24 48 64 128 256; do
    install -Dm644 "chrome/app/theme/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/chromium.png"
  done

  for size in 16 32; do
    install -Dm644 "chrome/app/theme/default_100_percent/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/chromium.png"
  done

  install -D "$srcdir/chromium.sh" "$pkgdir/usr/bin/chromium"

  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/chromium/LICENSE"
}

# vim:set ts=2 sw=2 et:
