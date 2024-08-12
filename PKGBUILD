# Maintainer: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=cromite
pkgver=127.0.6533.103
_pkgver=127.0.6533.102
_chrome_ver=127.0.6533.99
_commit=667d85d6eb35d8e2fe27b1ed3091ce4714eab4c8
pkgrel=1
_launcher_ver=8
_manual_clone=1
_system_clang=1
pkgdesc="A Bromite fork with ad blocking and privacy enhancements"
arch=('x86_64')
url="https://github.com/uazo/cromite"
license=('GPL3')
depends=('gtk3' 'nss' 'alsa-lib' 'xdg-utils' 'libxss' 'libcups' 'libgcrypt'
         'ttf-liberation' 'systemd' 'dbus' 'libpulse' 'pciutils' 'libva'
         'libffi' 'desktop-file-utils' 'hicolor-icon-theme')
makedepends=('python' 'gn' 'ninja' 'clang' 'lld' 'gperf' 'nodejs' 'pipewire'
             'rust' 'rust-bindgen' 'qt5-base' 'qt6-base' 'java-runtime-headless'
             'git')
optdepends=('pipewire: WebRTC desktop sharing under Wayland'
            'kdialog: support for native dialogs in Plasma'
            'gtk4: for --gtk-version=4 (GTK4 IME might work better on Wayland)'
            'org.freedesktop.secrets: password storage backend on GNOME / Xfce'
            'kwallet: support for storing passwords in KWallet on Plasma'
            'upower: Battery Status API support')
install="${pkgname}.install"
options=('!lto') # Chromium adds its own flags for ThinLTO
source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$_pkgver.tar.xz
        https://github.com/foutrelis/chromium-launcher/archive/v$_launcher_ver/chromium-launcher-$_launcher_ver.tar.gz
        https://github.com/uazo/cromite/archive/refs/tags/v$pkgver-$_commit.tar.gz
        https://dl.google.com/linux/deb/pool/main/g/google-chrome-stable/google-chrome-stable_$_chrome_ver-1_amd64.deb
        widevine-revision.patch
        chromium-browser-ui-missing-deps.patch
        compiler-rt-adjust-paths.patch
        increase-fortify-level.patch
        use-oauth2-client-switches-as-default.patch)
sha256sums=('61f0b41fa237922996c995b089df58117a7459b4c294d17e6f1cd2c6a7d2095e'
            '213e50f48b67feb4441078d50b0fd431df34323be15be97c55302d3fdac4483a'
            'c2a569ffb1ad23c668c15f21b2905f6b6925d8a60cc052aa8951388a14da31c6'
            'a4c18b49a8b80bf5e27c5911994a13446ff77444c62575a125b7b0b5bfeccd58'
            '474d900145ae6561220b550f1360fdc5c33e46b49e411e42d40799758a9b9565'
            '75f9c3ccdcc914d029ddcc5ca181df90177db35a343bf44ff541ff127bcea43d'
            'b3de01b7df227478687d7517f61a777450dca765756002c80c4915f271e2d961'
            'd634d2ce1fc63da7ac41f432b1e84c59b7cceabf19d510848a7cff40c8025342'
            'a9b417b96daec33c9059065e15b3a92ae1bf4b59f89d353659b335d9e0379db6')

if (( _manual_clone )); then
  source[0]=fetch-chromium-release
  sha256sums[0]=61f0b41fa237922996c995b089df58117a7459b4c294d17e6f1cd2c6a7d2095e
fi

# Possible replacements are listed in build/linux/unbundle/replace_gn_files.py
# Keys are the names in the above script; values are the dependencies in Arch
declare -gA _system_libs=(
  [brotli]=brotli
  #[dav1d]=dav1d
  #[ffmpeg]=ffmpeg    # YouTube playback stopped working in Chromium 120
  [flac]=flac
  [fontconfig]=fontconfig
  [freetype]=freetype2
  [harfbuzz-ng]=harfbuzz
  [icu]=icu
  #[jsoncpp]=jsoncpp  # needs libstdc++
  #[libaom]=aom
  #[libavif]=libavif  # needs https://github.com/AOMediaCodec/libavif/commit/5410b23f76
  [libdrm]=
  [libjpeg]=libjpeg
  [libpng]=libpng
  #[libvpx]=libvpx
  [libwebp]=libwebp
  [libxml]=libxml2
  [libxslt]=libxslt
  [opus]=opus
  #[re2]=re2          # needs libstdc++
  #[snappy]=snappy    # needs libstdc++
  #[woff2]=woff2      # needs libstdc++
  [zlib]=minizip
)
_unwanted_bundled_libs=(
  $(printf "%s\n" ${!_system_libs[@]} | sed 's/^libjpeg$/&_turbo/')
)
depends+=(${_system_libs[@]})

# Google API keys (see https://www.chromium.org/developers/how-tos/api-keys)
#
# Starting with Chromium 89 (2021-03-02) the OAuth2 credentials have been left
# out: https://archlinux.org/news/chromium-losing-sync-support-in-early-march/
_google_api_key=AIzaSyCkfPOPZXDKNn8hhgu3JrA62wIgC93d44k
_google_default_client_id=77185425430.apps.googleusercontent.com
_google_default_client_secret=OTJgUOQcT7lO7GsGZq2G4IlT

prepare() {
  bsdtar -x --strip-components 4 -f data.tar.xz opt/google/chrome/WidevineCdm

  if (( _manual_clone )); then
    ./fetch-chromium-release $_pkgver
  fi
  cd chromium-$_pkgver

  # Allow building against system libraries in official builds
  sed -i 's/OFFICIAL_BUILD/GOOGLE_CHROME_BUILD/' \
    tools/generate_shim_headers/generate_shim_headers.py

  # https://crbug.com/893950
  sed -i -e 's/\<xmlMalloc\>/malloc/' -e 's/\<xmlFree\>/free/' \
         -e '1i #include <cstdlib>' \
    third_party/blink/renderer/core/xml/*.cc \
    third_party/blink/renderer/core/xml/parser/xml_document_parser.cc \
    third_party/libxml/chromium/*.cc

  pushd $srcdir/cromite-$pkgver-$_commit/build/patches
  # Enable reverse image search
  rm -f WIN-Disable-search-for-image.patch
  # Enable Google {Account, Translate}
  rm -f add-browser-policy.patch
  rm -f ungoogled-chromium-Disable-translate-integration.patch
  rm -f ungoogled-chromium-Disable-Gaia.patch
  rm -f Internal-firewall.patch
  rm -f Remove-GoogleAccountsPrivateApiHost.patch
  # Remove keyboard protection
  rm -f Keyboard-protection-flag.patch
  # Remove bundled ABP
  find . -iname "*eyeo*.patch" -type f -delete
  popd

  for patch in $(cat $srcdir/cromite-$pkgver-$_commit/build/cromite_patches_list.txt); do
    if [ -f $srcdir/cromite-$pkgver-$_commit/build/patches/$patch ]; then
      echo "Applying: $patch"
      git apply $srcdir/cromite-$pkgver-$_commit/build/patches/$patch
    fi
  done

  # Widevine fixes from Debian
  patch -Np1 -i $srcdir/widevine-revision.patch

  # https://issues.chromium.org/issues/351157339
  patch -Np1 -i $srcdir/chromium-browser-ui-missing-deps.patch

  # Allow libclang_rt.builtins from compiler-rt >= 16 to be used
  patch -Np1 -i $srcdir/compiler-rt-adjust-paths.patch

  # Increase _FORTIFY_SOURCE level to match Arch's default flags
  patch -Np1 -i $srcdir/increase-fortify-level.patch

  # Link to system tools required by the build
  mkdir -p third_party/node/linux/node-linux-x64/bin
  ln -sf /usr/bin/node third_party/node/linux/node-linux-x64/bin/
  ln -sf /usr/bin/java third_party/jdk/current/bin/

  # test deps are broken for ui/lens with system ICU
  # "//third_party/icu:icuuc_public" (taken from Gentoo ebuild)
  sed -i '/source_set("unit_tests") {/,/}/d' chrome/browser/ui/lens/BUILD.gn
  sed -i '/lens:unit_tests/d' chrome/test/BUILD.gn components/BUILD.gn

  # Remove bundled libraries for which we will use the system copies; this
  # *should* do what the remove_bundled_libraries.py script does, with the
  # added benefit of not having to list all the remaining libraries
  local _lib
  for _lib in ${_unwanted_bundled_libs[@]}; do
    find "third_party/$_lib" -type f \
      \! -path "third_party/$_lib/chromium/*" \
      \! -path "third_party/$_lib/google/*" \
      \! -path "third_party/harfbuzz-ng/utils/hb_scoped.h" \
      \! -regex '.*\.\(gn\|gni\|isolate\)' \
      -delete
  done

  ./build/linux/unbundle/replace_gn_files.py \
    --system-libraries "${!_system_libs[@]}"
}

build() {
  make CHROMIUM_NAME=cromite -C chromium-launcher-$_launcher_ver

  cd chromium-$_pkgver

  if (( _system_clang )); then
    export CC=clang
    export CXX=clang++
    export AR=ar
    export NM=nm
  else
    local _clang_path="$PWD/third_party/llvm-build/Release+Asserts/bin"
    export CC=$_clang_path/clang
    export CXX=$_clang_path/clang++
    export AR=$_clang_path/llvm-ar
    export NM=$_clang_path/llvm-nm
  fi

  local _flags=('target_os = "linux"')
  _flags+=$(cat $srcdir/cromite-$pkgver-$_commit/build/cromite.gn_args)
  _flags+=(
    'custom_toolchain="//build/toolchain/linux/unbundle:default"'
    'host_toolchain="//build/toolchain/linux/unbundle:default"'
    'symbol_level=0' # sufficient for backtraces on x86(_64)
    'treat_warnings_as_errors=false'
    'blink_enable_generated_code_formatting=false'
    'rtc_use_pipewire=true'
    'link_pulseaudio=true'
    'use_custom_libcxx=true' # https://github.com/llvm/llvm-project/issues/61705
    'use_sysroot=false'
    'use_system_libffi=true'
    'enable_widevine=true'
    'use_qt6=true'
    'moc_qt6_path="/usr/lib/qt6"'
    "google_api_key=\"$_google_api_key\""
    "google_default_client_id=\"$_google_default_client_id\""
    "google_default_client_secret=\"$_google_default_client_secret\""
  )

  if [[ -n ${_system_libs[icu]+set} ]]; then
    _flags+=('icu_use_data_file=false')
  fi

  if (( _system_clang )); then
     local _clang_version=$(
       clang --version | grep -m1 version | sed 's/.* \([0-9]\+\).*/\1/')

    _flags+=(
      'clang_base_path="/usr"'
      'clang_use_chrome_plugins=false'
      "clang_version=\"$_clang_version\""
      'chrome_pgo_phase=0' # needs newer clang to read the bundled PGO profile
    )

    # Allow the use of nightly features with stable Rust compiler
    # https://github.com/ungoogled-software/ungoogled-chromium/pull/2696#issuecomment-1918173198
    export RUSTC_BOOTSTRAP=1

    _flags+=(
      'rust_sysroot_absolute="/usr"'
      'rust_bindgen_root="/usr"'
      "rustc_version=\"$(rustc --version)\""
    )
  fi

  # ThinLTO is enabled by default
  CFLAGS+='   -march=native'
  CXXFLAGS+=' -march=native'

  # Facilitate deterministic builds (taken from build/config/compiler/BUILD.gn)
  CFLAGS+='   -Wno-builtin-macro-redefined'
  CXXFLAGS+=' -Wno-builtin-macro-redefined'
  CPPFLAGS+=' -D__DATE__=  -D__TIME__=  -D__TIMESTAMP__='

  # Do not warn about unknown warning options
  CFLAGS+='   -Wno-unknown-warning-option'
  CXXFLAGS+=' -Wno-unknown-warning-option'

  # Let Chromium set its own symbol level
  CFLAGS=${CFLAGS/-g }
  CXXFLAGS=${CXXFLAGS/-g }

  # https://github.com/ungoogled-software/ungoogled-chromium-archlinux/issues/123
  CFLAGS=${CFLAGS/-fexceptions}
  CFLAGS=${CFLAGS/-fcf-protection}
  CXXFLAGS=${CXXFLAGS/-fexceptions}
  CXXFLAGS=${CXXFLAGS/-fcf-protection}

  # This appears to cause random segfaults when combined with ThinLTO
  # https://bugs.archlinux.org/task/73518
  CFLAGS=${CFLAGS/-fstack-clash-protection}
  CXXFLAGS=${CXXFLAGS/-fstack-clash-protection}

  # https://crbug.com/957519#c122
  CXXFLAGS=${CXXFLAGS/-Wp,-D_GLIBCXX_ASSERTIONS}

  gn gen out/Release --args="${_flags[*]}"
  ninja -C out/Release chrome chrome_sandbox chromedriver.unstripped
}

package() {
  cd chromium-launcher-$_launcher_ver
  make PREFIX=/usr DESTDIR="$pkgdir" CHROMIUM_NAME=cromite install
  install -Dm644 LICENSE \
    "$pkgdir/usr/share/licenses/cromite/LICENSE.launcher"

  cd ../chromium-$_pkgver

  install -D out/Release/chrome "$pkgdir/usr/lib/cromite/cromite"
  # install -D out/Release/chromedriver.unstripped "$pkgdir/usr/bin/chromedriver"
  install -Dm4755 out/Release/chrome_sandbox "$pkgdir/usr/lib/cromite/chrome-sandbox"

  install -Dm644 chrome/installer/linux/common/desktop.template \
    "$pkgdir/usr/share/applications/cromite.desktop"
  install -Dm644 chrome/app/resources/manpage.1.in \
    "$pkgdir/usr/share/man/man1/cromite.1"
  sed -i \
    -e 's/@@MENUNAME@@/Cromite/g' \
    -e 's/@@PACKAGE@@/chromium/g' \
    -e 's/@@USR_BIN_SYMLINK_NAME@@/cromite/g' \
    "$pkgdir/usr/share/applications/cromite.desktop" \
    "$pkgdir/usr/share/man/man1/cromite.1"

  install -Dm644 chrome/installer/linux/common/chromium-browser/chromium-browser.appdata.xml \
    "$pkgdir/usr/share/metainfo/cromite.appdata.xml"
  sed -ni \
    -e 's/chromium-browser\.desktop/cromite.desktop/' \
    -e '/<update_contact>/d' \
    -e '/<p>/N;/<p>\n.*\(We invite\|Chromium supports Vorbis\)/,/<\/p>/d' \
    -e '/^<?xml/,$p' \
    "$pkgdir/usr/share/metainfo/cromite.appdata.xml"

  local toplevel_files=(
    chrome_100_percent.pak
    chrome_200_percent.pak
    chrome_crashpad_handler
    libqt5_shim.so
    libqt6_shim.so
    resources.pak
    snapshot_blob.bin

    # ANGLE
    libEGL.so
    libGLESv2.so

    # SwiftShader ICD
    libvk_swiftshader.so
    libvulkan.so.1
    vk_swiftshader_icd.json
  )

  if [[ -z ${_system_libs[icu]+set} ]]; then
    toplevel_files+=(icudtl.dat)
  fi

  cp "${toplevel_files[@]/#/out/Release/}" "$pkgdir/usr/lib/cromite/"
  install -Dm644 -t "$pkgdir/usr/lib/cromite/locales" out/Release/locales/*.pak

  for size in 24 48 64 128 256; do
    install -Dm644 "chrome/app/theme/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/cromite.png"
  done

  for size in 16 32; do
    install -Dm644 "chrome/app/theme/default_100_percent/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/cromite.png"
  done

  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/cromite/LICENSE"

  cp -a $srcdir/WidevineCdm "$pkgdir/usr/lib/cromite/"
  find "$pkgdir/usr/lib/cromite/WidevineCdm" -name '*.so' -exec chmod +x {} \;
  install -Dm644 $srcdir/WidevineCdm/LICENSE \
    "$pkgdir/usr/share/licenses/cromite/LICENSE.widevine"
}

# vim:set ts=2 sw=2 et:
