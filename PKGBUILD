# Maintainer: Archiv8 <archiv8@artisteducator.com>
# Contributor: Archiv8 <archiv8@artisteducator.com>

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
"MIT.md"
"README.md"
)
noextract=("$_relname-$pkgver.tgz")
# validpgpkeys=()
sha512sums=('e6c437e23d48ae5b23e93b78596472959ed453ed630f97acfcb7c3641669fcb5c3c02e25746a8aa4c982091e99d34c758d8335a61992e0cf9e1aa4dd6a1d2a91'
            '8e6dbae786773cb199b3da9233afcd62d2c8f1be3752faa44e078012113fa3fde87f5830d0007a2823980d1e1624f5df3cf3719690fbdb4d3e09c8859c890fb7'
            'cedf902ebc5ade3ede792b7e33ebeb8e6c468f2d03b4d5fc68d3ae0e419a3b05f1f438be311e0e7ce054509edde925ea77457b9760ff376b66fe28c98c2d3419'
            'd531d89c22f91b91d1ef25b5aa71eb6ac678c4941e924591400c5eb49d4bfb7e8d711746d1b5a189ded2e6f4aacd4630a443bc4ffde56a79e67bc78c640dd860'
            '0a033730e23dc9c031929022e9be6e6a0bae20229ce6f0ae9389494f563e3e31a2bc996c755af89ad9c45dffd8f32fd774947164afc44e8edb86a3f80a7e96b4'
            '29e7f844942245a67e2032481db6bb815d0a3d02cc499964c466bfe19896e24da7449c413f22e96b3e21ebdadd3176a241161c2572608e89483adc6df5aded77'
            'db78636dbf371579766e0252c135cdd61fec4dd9b581d1da76e13f63f895bda64712a29a53ea2b57b96b706f5a8b6511b2b3725698b9d0918895beb31003e0cf'
            '6521bd226b0167bc403571e3d0425b629b1ce0a40d9c45e70ccc62d09cf2a2be4195f73a8c7a36b86c9479c71d6684333e6769cde253300d7c886eea513687ee')

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
  install -Dm 644 "CC-by-SA-v4.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/CC-by-SA-v4.md"
  install -Dm 644 "CHANGELOG.md" "$pkgdir/usr/share/doc/$pkgname/packaging/CHANGELOG.md"
  install -Dm 644 "ISSUES.md" "$pkgdir/usr/share/doc/$pkgname/packaging/ISSUES.md"
  install -Dm 644 "LICENSE.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/LICENSE.md"
  install -Dm 644 "MIT.md" "$pkgdir/usr/share/licenses/$pkgname/packaging/MIT.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/$pkgname/packaging/README.md"
  }
