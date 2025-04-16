# CodingPlayground / Python / MinimalPython

https://devguide.python.org/getting-started/setup-building/#compile-and-build

## Download The Source Code
```bash
curl -O https://www.python.org/ftp/python/3.13.3/Python-3.13.3.tar.xz
tar -xf Python-3.13.3.tar.xz
rm Python-3.13.3.tar.xz
```

## Run Configuration
```bash
cd Python-3.13.3

./configure \
  --disable-shared \
  --enable-shared=no \
  --disable-ipv6 \
  --without-pymalloc \
  --without-doc-strings \
  --without-ensurepip \
  --with-ensurepip=no \
  --without-pydebug \
  --disable-test-modules \
  --with-static-libpython \
  --prefix=$(pwd)/minimal-python
```

## Exclude Modules

```bash
echo "*disabled*
_ssl _hashlib _sqlite3 _bz2 _lzma zlib _decimal _ctypes _uuid
_gdbm _dbm _curses _curses_panel readline _tkinter
_bz2 _lzma _blake2 _sha3 _md5 _sha1 _sha256 _sha512
_csv _json _heapq _bisect _random _statistics _multiprocessing _posixshmem
audioop mmap fcntl resource grp termios syslog
" > Modules/Setup.local
```

## Build Python

```bash
# produces ./python but no shared objects
make -j12 platform
make install
```