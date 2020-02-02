# Maintainer: Ross Clark <https://github.com/Archiv8/nodejs-remark-preset-lint-recommended/issues>
# Contributor: Ross Clark <https://github.com/Archiv8/nodejs-remark-preset-lint-recommended/issues>

_relname="remark-preset-lint-recommended"

# pkgbase=
pkgname="nodejs-${_relname}"
pkgver=3.0.3
pkgrel=1
# epoch=
pkgdesc="remark lint plugin with settings that help prevent mistakes and highlights syntaxes that do not work correctly across vendors."
arch=("any")
url="https://github.com/remarkjs/remark-lint/tree/master/packages/remark-preset-lint-recommended"
license=("MIT")
# groups=()
depends=("nodejs")
# optdepends=()
makedepends=("jq" "npm")
# checkdepends=()
# provides=()
# conflicts=()
# replaces=()
# backup=()
# options=()
# install=
changelog="CHANGELOG.md"
source=(
"https://registry.npmjs.org/$_relname/-/$_relname-$pkgver.tgz"
"CC-by-SA-v4.md"
"CHANGELOG.md"
"ISSUES.md"
"LICENSE"
"LICENSE.md"
"README.md"
)
# noextract=()
# validpgpkeys=()
sha256sums=('7c43a4ed3efc40b8ddc43930f83eba4e0392fb26df2e8ec2f8fff454a229e5ee'
            'c492f31eb0daf519a3c8ab2db7899806de693608f3894d9c82ee5f0a079b603f'
            '37fecd09f29ad9d1aaa81e3e7aac939e33ce9b044a00e3eb3ab95b6a8cce5f70'
            'a8f44e8684e920ea4becde607f6a8315083e75fb23fd3632c4c819aa73e36340'
            'ed52f87c3904a045931ff47fef50c31840b2f80f3618ce7a9d188918a9f83357'
            '99e8d4a1e54d7245fc77bd46a88226c6b52d0dd45cdbe32f9a33434c6f3c0a82'
            '20cd8f66f7874e89be5c7d7aa7082cd3c3421ec10ecea68ac48064e07759279a')

# prepare () {}

# build() {}

# check() {}

package() {
  # Ensure cache is set when install is run in order to avoid littering user's home directory
  printf "\e[1;32m==>\e[0m \e[38;5;248mBuilding nodejs package... \e[0m\n"
  printf "    This may take some time depending on the size of the package and number of dependencies to download... \e[0m\n"
  npm install --cache "$srcdir/npm-cache" -g --user root --prefix "$pkgdir"/usr "$srcdir"/$_relname-$pkgver.tgz

  # Fix incorrect user / group as highlighted by namcap
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible ownership issues\e[0m\n"
  while IFS= read -r fsitem; do
    chown -h root:root "$fsitem"
  done <   <(find "$pkgdir" -not -group root -not -user root)

  # Non-deterministic race in npm gives 777 permissions to random directories.
  # See https://github.com/npm/npm/issues/9359 for details.
  printf "\e[1;32m==>\e[0m \e[38;5;248mCorrecting possible permission issues on directories\e[0m\n"
  find "$pkgdir/usr" -type d -exec chmod 755 '{}' +

  # Remove references to $pkgdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$pkgdir\e[0m\n"
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  printf "\e[1;32m==>\e[0m \e[38;5;248mRemoving references to \$srcdir \e[0m\n"
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$_relname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"

  # Install license
  install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

  # Create Archiv8 documentation folder
  install -dvm 755 "$pkgdir/usr/share/doc/$pkgname/packaging/"

  # Install Archiv8 Documentation
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/doc/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
