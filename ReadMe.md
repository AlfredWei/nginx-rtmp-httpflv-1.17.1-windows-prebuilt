
# Nginx 1.17.1 for windows 

## Diff
   - Use http-flv instead of rtmp
   
## Steps

- Install ActivePerl https://www.activestate.com/products/activeperl/downloads/
- Install VC https://visualstudio.microsoft.com/vs/community/?rr=https%3A%2F%2Fwww.google.com%2F
- Install Windows SDK (ex: 10.0.18362.0)
- Install NASM https://www.nasm.us/pub/nasm/releasebuilds/2.14.02/win32/
- Install MSYS or use CMDer, here we use cmder  https://cmder.net/
- Put init.cmd in CMDer
```cmd
# this helps you init VC env under cmder
call "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC\Auxiliary\Build\vcvarsall.bat" x86_amd64 10.0.18362.0
set path=C:\Users\alfred.wei\AppData\Local\bin\NASM;%path%

```

- go to nginx src folder
- mkdir obj
- get pcre (not pcre2) from http://www.pcre.org/, extract to obj/lib
- get openssl from https://www.openssl.org/, extract to obj/lib
- get zlib from http://zlib.net/, extract to obj/lib
- get nginx http-flv, git clone into obj/lib, nginx-http-flv-module包含了nginx-rtmp-module所有的功能，所以不要将nginx-http-flv-module和nginx-rtmp-module一起编译。
- 修改錯誤級別為3級，d:\git\nginx\auto\cc\msvc的83行，把 -W4 修改為 -W3

- go back to nginx src folder

```sh
bash 
auto/configure --with-cc=cl --builddir=objs --prefix= --conf-path=conf/nginx.conf --pid-path=logs/nginx.pid --http-log-path=logs/access.log --error-log-path=logs/error.log --sbin-path=nginx.exe --http-client-body-temp-path=temp/client_body_temp --http-proxy-temp-path=temp/proxy_temp --http-fastcgi-temp-path=temp/fastcgi_temp --http-scgi-temp-path=temp/scgi_temp --http-uwsgi-temp-path=temp/uwsgi_temp --with-cc-opt=-DFD_SETSIZE=1024 --with-pcre=objs/lib/pcre-8.43 --with-zlib=objs/lib/zlib1211 --with-select_module --with-http_v2_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_stub_status_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_slice_module --with-mail --with-stream --with-openssl=objs/lib/openssl-1.1.1c --with-http_ssl_module --with-mail_ssl_module --with-stream_ssl_module --add-module=objs/lib/nginx-http-flv-module
```

- leave bash
- nmake -f obj/Makefile


