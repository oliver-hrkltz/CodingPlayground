# CodingPlayground / Lua / MinimalLua

## Download Source Code

```bash
curl -R -O https://www.lua.org/ftp/lua-5.4.7.tar.gz
tar -xzf lua-5.4.7.tar.gz
rm lua-5.4.7.tar.gz
cd lua-5.4.7
```

## Strip Down

Remove some packages:

- liolib.o (IO)
- loslib.o (OS)
- loadlib.o (package/require)

```bash
echo "
--- lua-5.4.7/src/Makefile
+++ lua-5.4.7/src/Makefile
@@ -34,7 +34,7 @@
 
 LUA_A=	liblua.a
 CORE_O=	lapi.o lcode.o lctype.o ldebug.o ldo.o ldump.o lfunc.o lgc.o llex.o lmem.o lobject.o lopcodes.o lparser.o lstate.o lstring.o ltable.o ltm.o lundump.o lvm.o lzio.o
-LIB_O=	lauxlib.o lbaselib.o lcorolib.o ldblib.o liolib.o lmathlib.o loadlib.o loslib.o lstrlib.o ltablib.o lutf8lib.o linit.o
+LIB_O=	lauxlib.o lbaselib.o lcorolib.o ldblib.o lmathlib.o lstrlib.o ltablib.o lutf8lib.o linit.o
 BASE_O= \$(CORE_O) \$(LIB_O) \$(MYOBJS)
 
 LUA_T=	lua" > a.patch

echo "
--- lua-5.4.7/src/linit.c
+++ lua-5.4.7/src/linit.c
@@ -41,15 +41,9 @@
 */
 static const luaL_Reg loadedlibs[] = {
   {LUA_GNAME, luaopen_base},
-  {LUA_LOADLIBNAME, luaopen_package},
-  {LUA_COLIBNAME, luaopen_coroutine},
   {LUA_TABLIBNAME, luaopen_table},
-  {LUA_IOLIBNAME, luaopen_io},
-  {LUA_OSLIBNAME, luaopen_os},
   {LUA_STRLIBNAME, luaopen_string},
   {LUA_MATHLIBNAME, luaopen_math},
-  {LUA_UTF8LIBNAME, luaopen_utf8},
-  {LUA_DBLIBNAME, luaopen_debug},
   {NULL, NULL}
 };
" > b.patch

patch -p0 < a.patch
patch -p0 < b.patch
```

## Build

```bash
cd lua-5.4.7/src
make macosx
```

## Test

```bash
cd lua-5.4.7/src
./lua -e 'print("√144 =", math.sqrt(144))' # √144 = 12
./lua -e 'print("io is", io)' # io is	nil
```

