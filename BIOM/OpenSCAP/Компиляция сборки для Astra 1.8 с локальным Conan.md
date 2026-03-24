# 1) пакеты
sudo apt update
sudo apt install -y \
  build-essential cmake git pkg-config make g++ gcc-11 g++-11 \
  python3 python3-venv python3-pip perl sed ca-certificates \
  libdbus-1-dev libdbus-glib-1-dev libcurl4-openssl-dev libgcrypt20-dev \
  libselinux1-dev libxslt1-dev libacl1-dev libblkid-dev libcap-dev \
  libxml2-dev libldap2-dev libpcre3-dev libxml-parser-perl libxml-xpath-perl \
  libperl-dev libbz2-dev librpm-dev libapt-pkg-dev libyaml-dev \
  libxmlsec1-dev libxmlsec1-openssl parted

# 2) git ssl workaround при необходимости
git config --global http."https://azure.da.lan/".sslVerify false

# 3) clone
git clone --recurse-submodules --branch remote --single-branch https://azure.da.lan/
cd openscap

# 4) venv + conan1
python3 -m venv .venv
. .venv/bin/activate
pip install "conan<2"

# 5) profile gcc11
conan profile new default --detect --force
conan profile update settings.compiler=gcc default
conan profile update settings.compiler.version=11 default
conan profile update settings.compiler.libcxx=libstdc++11 default
conan profile update settings.build_type=Release default

# 6) private conan
conan remote add private-conan "https://conan.lan/artifactory/api/conan/" False --force --insert
conan user "guest" -r private-conan -p

# 7) verify deps
conan search -r private-conan "boost/1.86.0"
conan search -r private-conan "pcre/8.45ci1"
conan search -r private-conan "libssh/0.11.1"
conan search -r private-conan "m4/1.4.18@"
conan search -r private-conan "libtool/2.4.7ci1@"

# 8) pre-download troublesome packages
conan download "m4/1.4.18@" -r private-conan
conan download "libtool/2.4.7ci1@" -r private-conan

# 9) libtool workaround
sudo mkdir -p /home/ladmin
sudo ln -sfn /home/administrator/.conan /home/ladmin/.conan

# 10) ВНЕСТИ 2 ПАТЧА В ИСХОДНИКИ:

# 11) build
rm -rf build
mkdir build
cd build

export CC=/usr/bin/gcc-11
export CXX=/usr/bin/g++-11

cmake .. \
  -DCMAKE_INSTALL_PREFIX=/opt/oscap-custom \
  -DCMAKE_C_COMPILER=/usr/bin/gcc-11 \
  -DCMAKE_CXX_COMPILER=/usr/bin/g++-11 \
  -DCMAKE_C_FLAGS="-include limits.h"

cmake --build . -j"$(nproc)"

# 12) install
sudo cmake --install .

# 13) runtime env
export LD_LIBRARY_PATH=/opt/oscap-custom/lib:$LD_LIBRARY_PATH
export OSCAP_SCHEMA_PATH=/opt/oscap-custom/share/openscap/schemas
export OSCAP_XSLT_PATH=/opt/oscap-custom/share/openscap/xsl
export OSCAP_CPE_PATH=/opt/oscap-custom/share/openscap/cpe

# 14) test
/opt/oscap-custom/bin/oscap --version
/opt/oscap-custom/bin/oscap oval validate /tmp/eltex-oval-file.xml
/opt/oscap-custom/bin/oscap oval eval --results /tmp/results-config.xml /tmp/eltex-oval-file.xml