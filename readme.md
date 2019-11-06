Modify the `_WIN32_WINNT` define to `0x0501`

`cmake .. -DCMAKE_TOOLCHAIN_FILE=../CMakeLists_mingw.txt -G Ninja -DCMAKE_CXX_FLAGS="-fdiagnostics-color=always -Wno-error=cast-function-type -DCURL_STATICLIB=1 -DZIP_STATIC=1 -static" -DCMAKE_BUILD_TYPE=relwithdebinfo -DSTATIC=on` and then bunch of fixing linkage issues.

Latest (relevant part) incantation was:
`-o openrct2.exe -Wl,--out-implib,libopenrct2.dll.a -Wl,--major-image-version,0,--minor-image-version,0  -static libopenrct2.a -L/usr/i686-w64-mingw32/lib -lmingw32 /usr/i686-w64-mingw32/lib/libSDL2main.a /usr/i686-w64-mingw32/lib/libSDL2-static.a -mwindows -L/usr/i686-w64-mingw32/lib /usr/i686-w64-mingw32/lib/libspeexdsp.a -lgdi32 -lopengl32 -lcomdlg32 /usr/i686-w64-mingw32/lib/libwinpthread.a -lws2_32 -lcrypt32 -lwldap32 -lversion -lwinmm -limm32 -ladvapi32 -lshell32 -lole32 /usr/i686-w64-mingw32/lib/libcurl.a /usr/i686-w64-mingw32/lib/libidn2.a -lssh2 /usr/i686-w64-mingw32/lib/libpsl.a /usr/i686-w64-mingw32/lib/libssl.a /usr/i686-w64-mingw32/lib/libcrypto.a -lz /usr/i686-w64-mingw32/lib/libjansson.a /usr/i686-w64-mingw32/lib/libpng16.a /usr/i686-w64-mingw32/lib/libzip.a /usr/i686-w64-mingw32/lib/libbz2.a /usr/i686-w64-mingw32/lib/libfreetype.a -lws2_32 -lcrypt32 -lwldap32 -lversion -lwinmm -limm32 -ladvapi32 -lshell32 -lole32 /usr/i686-w64-mingw32/lib/libcurl.a /usr/i686-w64-mingw32/lib/libidn2.a  /usr/i686-w64-mingw32/lib/libssh2.a /usr/i686-w64-mingw32/lib/libpsl.a /usr/i686-w64-mingw32/lib/libssl.a /usr/i686-w64-mingw32/lib/libcrypto.a /usr/i686-w64-mingw32/lib/libz.a /usr/i686-w64-mingw32/lib/libjansson.a /usr/i686-w64-mingw32/lib/libpng16.a /usr/i686-w64-mingw32/lib/libzip.a /usr/i686-w64-mingw32/lib/libbz2.a /usr/i686-w64-mingw32/lib/libfreetype.a -lgdi32 -lkernel32 -luser32 -lgdi32 -lwinspool -lshell32 -lole32 -loleaut32 -luuid -lcomdlg32 -ladvapi32 -lsetupapi -static-libgcc -static-libstdc++ /usr/i686-w64-mingw32/lib/libunistring.a /usr/i686-w64-mingw32/lib/libiconv.a  /usr/i686-w64-mingw32/lib/libintl.a /usr/i686-w64-mingw32/lib/libssp.a -static`

Transplant the resulting exe to one of the CI-built archives

Move the debug info out of the file:
`i686-w64-mingw32-objcopy --only-keep-debug openrct2.exe openrct2.exe.dbg`
`i686-w64-mingw32-objcopy --strip-debug openrct2.exe`
