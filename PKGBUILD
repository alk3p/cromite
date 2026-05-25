# Maintainer: Christian Heusel <gromit@archlinux.org>
# Contributor: Evangelos Foutras <foutrelis@archlinux.org>
# Contributor: Pierre Schmitz <pierre@archlinux.de>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=cromite
pkgver=148.0.7778.168
_pkgver=148.0.7778.167
_chrome_ver=$_pkgver
_commit=cb3baf14f52eb4365d017f640f85310735c19b79
pkgrel=1
_launcher_ver=8
_manual_clone=1
_system_clang=1
pkgdesc="A Bromite fork with ad blocking and privacy enhancements"
arch=('x86_64')
url="https://github.com/uazo/cromite"
license=('GPL3')
depends=(
  'alsa-lib'
  'dbus'
  'desktop-file-utils'
  'gtk3'
  'hicolor-icon-theme'
  'libcups'
  'libffi'
  'libgcrypt'
  'libpulse'
  'libva'
  'libxss'
  'nss'
  'pciutils'
  'systemd'
  'ttf-liberation'
  'xdg-utils'
)
makedepends=(
  'clang'
  'compiler-rt'
  'git'
  'gn'
  'gperf'
  'java-runtime-headless'
  'lld'
  'ninja'
  'nodejs'
  'pipewire'
  'python'
  'qt6-base'
  'rust-bindgen'
  'rust'
)
optdepends=('pipewire: WebRTC desktop sharing under Wayland'
            'kdialog: support for native dialogs in Plasma'
            'gtk4: for --gtk-version=4 (GTK4 IME might work better on Wayland)'
            'org.freedesktop.secrets: password storage backend on GNOME, KDE and Xfce'
            'upower: Battery Status API support')
install="${pkgname}.install"
options=('!lto') # Chromium adds its own flags for ThinLTO
source=(https://commondatastorage.googleapis.com/chromium-browser-official/chromium-$_pkgver-lite.tar.xz
        https://github.com/foutrelis/chromium-launcher/archive/v$_launcher_ver/chromium-launcher-$_launcher_ver.tar.gz
        https://github.com/uazo/cromite/archive/refs/tags/v$pkgver-$_commit.tar.gz
        https://dl.google.com/linux/deb/pool/main/g/google-chrome-stable/google-chrome-stable_$_chrome_ver-1_amd64.deb
        b-505023370.patch
        cromite-fixup-Enable-component-updater.patch
        widevine-revision.patch
        chromium-138-nodejs-version-check.patch
        chromium-145-fix-SYS_SECCOMP.patch
        chromium-146-drop-unknown-clang-flag.patch
        chromium-146-build-with-wasm-rollup.patch
        chromium-147-revert-clang-no-lifetime-dse-flag.patch
        chromium-147-rust-1.95-bytemuck.patch
        chromium-148-revert-clang-fsanitize-return-flag-1.patch
        chromium-148-revert-clang-fsanitize-return-flag-2.patch
        compiler-rt-adjust-paths.patch
        increase-fortify-level.patch
        enable-widevine-arm64.patch
        use-oauth2-client-switches-as-default.patch
        glibc-2.42-baud-rate-fix.patch)
sha256sums=('57a78706c149afe8cacdc801c20ef89ad1c1fbba0ad2154fda03f4fc0f33c9d6'
            '213e50f48b67feb4441078d50b0fd431df34323be15be97c55302d3fdac4483a'
            '7406201bc59024d7d918ffebef3c874313c27b6d0ae3b84486b914fd53d8d088'
            'd64f4facad66a5424527a45b0b76443740aa38b5f825b1a99a95d0ac3a594d55'
            '0947b0bae80dec3bca959ec66a4a82c07609c7614068b61583d6fdd27a61ffd4'
            '8ffc34510cc73475aad54c1bc49f618c9b1eb4ac77984072d02f0de71a9cb9d3'
            'e9f6c962dcc5bbef3120004de8f4b29b09f0f74d16a272c0a704ef485c52441a'
            '11a96ffa21448ec4c63dd5c8d6795a1998d8e5cd5a689d91aea4d2bdd13fb06e'
            '4fc040a0656a0a524dd8ad090cd129fc5b6cb21adcc66be82080165789e8c13e'
            '4bf6baedb6d9a84b98a85584981f4d2db1ea91f5596f44d700027b8cdbf1ecbb'
            '45fa20cc27ef0aa00d654d0bac84bfaa8d8090b5f8aec49cc2e8d7249d3cd7ba'
            'c382830318c5b37826ecf44f3ba9def6be8affdad1bce819ecb83f3222ff4b3a'
            'b9e6339221efe03540ffb360c161d93604a1fc93a5a1c53e5e9849066f987d05'
            '2c0d0407ff7d4d607cf4f4b56aef4913df1bcbacb630d85c06a4a125fd0dceab'
            '7836f666b78b85ac4a05cc9403df74c80d17f18a7f2a29d489848c76db919128'
            'ec8e49b7114e2fa2d359155c9ef722ff1ba5fe2c518fa48e30863d71d3b82863'
            'd634d2ce1fc63da7ac41f432b1e84c59b7cceabf19d510848a7cff40c8025342'
            '9c766b82d1143cb3413fe2057361bd2655e46287eacc2c6d6f8504b4c255647a'
            '9343afa1a4308a7cfb3317229f5aff7778688debcc03c4a74a85908aa1d0cc3a'
            '1c1898f263eaacbc069a8e1a3e732852350350d1dad4cb1a6bba430e3b796cd0')

if (( _manual_clone )); then
  source[0]=fetch-chromium-release
  sha256sums[0]='380ef492e5a347219d5ea2755a24625993eed65fc2951d5e6c31dd229edd0227'
  makedepends+=('python-httplib2' 'python-pyparsing' 'python-six' 'npm' 'rsync')
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
  [harfbuzz]=harfbuzz
  #[icu]=icu
  #[jsoncpp]=jsoncpp  # needs libstdc++
  #[libaom]=aom
  #[libavif]=libavif  # needs -DAVIF_ENABLE_EXPERIMENTAL_GAIN_MAP=ON
  [libdrm]=libdrm
  [libjpeg]=libjpeg-turbo
  #[libpng]=libpng
  #[libvpx]=libvpx
  [libwebp]=libwebp
  [libxml]=libxml2
  [libxslt]=libxslt
  [openh264]=openh264
  [opus]=opus
  #[re2]=re2          # needs libstdc++
  #[snappy]=snappy    # needs libstdc++
  #[woff2]=woff2      # needs libstdc++
  [zlib]=minizip
  [zstd]=zstd
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
  # rustup install nightly
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
  # Restore default codecs
  rm -f Enable-platform-aac-audio-and-h264-video.patch
  # Enable reverse image search
  rm -f WIN-Disable-search-for-image.patch
  # Enable Google {Account, Translate}
  rm -f add-browser-policy.patch
  #rm -f ungoogled-chromium-Disable-translate-integration.patch
  rm -f ungoogled-chromium-Disable-Gaia.patch
  rm -f Internal-firewall.patch
  rm -f Remove-GoogleAccountsPrivateApiHost.patch
  # Remove bundled ABP
  find . -iname "*eyeo*.patch" -type f -delete
  popd

  for patch in $(cat $srcdir/cromite-$pkgver-$_commit/build/cromite_patches_list.txt); do
    if [ -f $srcdir/cromite-$pkgver-$_commit/build/patches/$patch ]; then
      echo "Applying: $patch"
      git apply $srcdir/cromite-$pkgver-$_commit/build/patches/$patch
    fi
  done
  patch -Np1 -i $srcdir/cromite-fixup-Enable-component-updater.patch

  # Widevine fixes from Debian
  patch -Np1 -i $srcdir/widevine-revision.patch

  # Upstream fixes
  patch -Np1 -i $srcdir/b-505023370.patch

  # Fixes from Gentoo
  patch -Np1 -i $srcdir/chromium-138-nodejs-version-check.patch

  # Allow libclang_rt.builtins from compiler-rt >= 16 to be used
  patch -Np1 -i $srcdir/compiler-rt-adjust-paths.patch

  # Increase _FORTIFY_SOURCE level to match Arch's default flags
  patch -Np1 -i $srcdir/increase-fortify-level.patch

  # clang 22 lacks -fsanitize-ignore-for-ubsan-feature, which is needed to use
  # -fsanitize=array-bounds without triggering UBSan feature detection. Without
  # feature detection suppression, V8 compiles in __sanitizer_set_death_callback
  # calls that require the UBSan runtime, which is not linked in a trap-mode
  # build. Drop the entire sanitize_c_array_bounds cflags block.
  # Can be dropped when arch has LLVM 23.
  patch -Np1 -i $srcdir/chromium-146-drop-unknown-clang-flag.patch

  # Causes a build failure with our clang version
  patch -Np1 -i $srcdir/chromium-147-revert-clang-no-lifetime-dse-flag.patch

  # https://crbug.com/456218403
  patch -Np1 -i $srcdir/chromium-145-fix-SYS_SECCOMP.patch

  patch -Np1 -i $srcdir/chromium-146-build-with-wasm-rollup.patch

  patch -Np1 -i $srcdir/chromium-147-rust-1.95-bytemuck.patch

  # enable widevine for arm64
  patch -Np1 -i $srcdir/enable-widevine-arm64.patch

  # https://crbug.com/456677057
  patch -Np1 -i $srcdir/glibc-2.42-baud-rate-fix.patch

  # Causes a build failure with our clang version
  patch -Np1 -i $srcdir/chromium-148-revert-clang-fsanitize-return-flag-1.patch
  patch -Np1 -i $srcdir/chromium-148-revert-clang-fsanitize-return-flag-2.patch

  # Link to system tools required by the build
  mkdir -p third_party/node/linux/node-linux-x64/bin \
           third_party/rust-toolchain/bin \
           third_party/jdk/current/bin

  ln -sf /usr/bin/node third_party/node/linux/node-linux-x64/bin/
  ln -sf /usr/bin/java third_party/jdk/current/bin/

  # remove x86_64 binary and use our own
  rm -f third_party/gperf/cipd/bin/gperf
  ln -s /usr/bin/gperf third_party/gperf/cipd/bin/

  if (( !_system_clang )); then
    # Use prebuilt rust as system rust cannot be used due to the error:
    #   error: the option `Z` is only accepted on the nightly compiler
    ./tools/rust/update_rust.py

    # To link to rust libraries we need to compile with prebuilt clang
    ./tools/clang/scripts/update.py
  fi

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

  # Generate missing header
  python3 build/util/lastchange.py -m DAWN_COMMIT_HASH \
    -s third_party/dawn --revision gpu/webgpu/DAWN_VERSION \
    --header gpu/webgpu/dawn_commit_hash.h
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

  local _flags=(
    'custom_toolchain="//build/toolchain/linux/unbundle:default"'
    'host_toolchain="//build/toolchain/linux/unbundle:default"'
    'is_official_build=true' # implies is_cfi=true on x86_64
    'symbol_level=0' # sufficient for backtraces on x86(_64)
    'treat_warnings_as_errors=false'
    'fatal_linker_warnings=false'
    'disable_fieldtrial_testing_config=true'
    'blink_enable_generated_code_formatting=false'
    'ffmpeg_branding="Chrome"'
    'proprietary_codecs=true'
    'rtc_use_pipewire=true'
    'link_pulseaudio=true'
    'use_custom_libcxx=true' # https://github.com/llvm/llvm-project/issues/61705
    'use_sysroot=false'
    'use_system_libffi=true'
    'enable_hangout_services_extension=true'
    'enable_widevine=true'
    'enable_nacl=false'
    'use_qt5=false'
    'use_qt6=true'
    'moc_qt6_path="/usr/lib/qt6"'
    "google_api_key=\"$_google_api_key\""
    'use_clang_modules=false'
    "google_default_client_id=\"$_google_default_client_id\""
    "google_default_client_secret=\"$_google_default_client_secret\""
  )
  _flags+=(
    'enable_mdns=false'
    'enable_reporting=false'
    'is_component_build=false'
    'enable_bound_session_credentials=false'
    'use_rtti=false'
    'chrome_pgo_phase=2'
    'enable_glic=false'
    'build_tflite_with_xnnpack=false'
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
      "rustc_version=\"$(rustc --version | awk '{ print $2 ;}')\""
    )
  fi

  # ThinLTO is enabled by default
  CFLAGS+='   -march=x86-64-v3 -O3'
  CXXFLAGS+=' -march=x86-64-v3 -O3'

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

  if [[ $CARCH == aarch64 ]] || [[ $CARCH == riscv64 ]]; then
    # On aarch64 and riscv64, certain files (e.g. in libvpx and libyuv) needs to
    # be compiled with additional arch features (e.g. dotprod, sve, sme, rvv)
    # Having an arch setting in the C(XX)FLAGS overrides those
    # and causes compilation failure
    CFLAGS="${CFLAGS/-march=*([^ ]) }"
    CXXFLAGS="${CXXFLAGS/-march=*([^ ]) }"
  fi

  gn gen out/Release --args="${_flags[*]}"
  ninja -C out/Release chrome chrome_sandbox chromedriver.unstripped
}

package() {
  cd chromium-launcher-$_launcher_ver
  make PREFIX=/usr DESTDIR="$pkgdir" CHROMIUM_NAME=cromite install
  install -Dvm644 LICENSE \
    "$pkgdir/usr/share/licenses/cromite/LICENSE.launcher"

  cd ../chromium-$_pkgver

  install -Dv out/Release/chrome "$pkgdir/usr/lib/cromite/cromite"
  # install -Dv out/Release/chromedriver.unstripped "$pkgdir/usr/bin/chromedriver"
  install -Dvm4755 out/Release/chrome_sandbox "$pkgdir/usr/lib/cromite/chrome-sandbox"

  install -Dvm644 chrome/installer/linux/common/desktop.template \
    "$pkgdir/usr/share/applications/cromite.desktop"
  install -Dvm644 chrome/app/resources/manpage.1.in \
    "$pkgdir/usr/share/man/man1/cromite.1"
  sed -i \
    -e 's/@@MENUNAME/Cromite/g' \
    -e 's/@@PACKAGE/chromium/g' \
    -e 's/@@usr_bin_symlink_name/cromite/g' \
    -e 's|@@uri_scheme|x-scheme-handler/chromium;|g' \
    -e 's/@@extra_desktop_entries//g' \
    "$pkgdir/usr/share/applications/cromite.desktop" \
    "$pkgdir/usr/share/man/man1/cromite.1"

  # Fill in common Chrome/Chromium AppData template with Chromium info
  (
    tmpl_file=chrome/installer/linux/common/appdata.xml.template
    info_file=chrome/installer/linux/common/chromium-browser.info
    . $info_file; PACKAGE=cromite
    export $(grep -o '^[A-Z_]*' $info_file)
    sed -E -e 's/@@([A-Z_]*)@@/\${\1}/g' -e '/<update_contact>/d' $tmpl_file | envsubst
  ) \
  | install -Dvm644 /dev/stdin "$pkgdir/usr/share/metainfo/cromite.appdata.xml"

  local toplevel_files=(
    chrome_100_percent.pak
    chrome_200_percent.pak
    chrome_crashpad_handler
    libqt6_shim.so
    resources.pak
    v8_context_snapshot.bin

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
  install -Dvm644 -t "$pkgdir/usr/lib/cromite/locales" out/Release/locales/*.pak

  for size in 24 48 64 128 256; do
    install -Dvm644 "chrome/app/theme/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/cromite.png"
  done

  for size in 16 32; do
    install -Dvm644 "chrome/app/theme/default_100_percent/chromium/product_logo_$size.png" \
      "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/cromite.png"
  done

  install -Dvm644 LICENSE "$pkgdir/usr/share/licenses/cromite/LICENSE"

  cp -a $srcdir/WidevineCdm "$pkgdir/usr/lib/cromite/"
  find "$pkgdir/usr/lib/cromite/WidevineCdm" -name '*.so' -exec chmod +x {} \;
  install -Dvm644 $srcdir/WidevineCdm/LICENSE \
    "$pkgdir/usr/share/licenses/cromite/LICENSE.widevine"
}

# vim:set ts=2 sw=2 et:
