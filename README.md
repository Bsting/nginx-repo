Compile ngx_http_js_module for cflinuxfs3

Option 1: Compile from NGinx Source with CloudFoundry docker image
------------------------------------------------------------------------
1. Get nginx source from https://github.com/nginx
2. Get njs source from https://github.com/nginx/njs
3. Copy njs source folder to Nginx source folder
4. Run cloudfoundry/cflinuxfs in it mode

   _docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs3 bash _
   
6. Configure nginx

   _./auto/configure --sbin-path=/usr/local/nginx/nginx --conf-path=/usr/local/nginx/nginx.conf --pid-path=/usr/local/nginx/nginx.pid --error-log-path=stderr --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --without-http_uwsgi_module --without-http_scgi_module --with-pcre --with-pcre-jit --without-pcre2 --with-cc-opt='-fPIC -pie' --with-ld-opt='-fPIC -pie -z now' --with-compat --with-stream=dynamic --with-http_sub_module --add-dynamic-module='/binary-builder/njs/nginx'_
   
8. Compile using make

   _make_
   
10. Get ngx_http_js_module.so from objs folder

Option 2: Compile from CloudFoundry binary-builder with CloudFoundry docker image
---------------------------------------------------------------------------------------
1. Get binary-builder source from https://github.com/cloudfoundry/binary-builder
2. Get njs source from https://github.com/nginx/njs
3. Copy njs source folder to binary-builder source folder
4. Update computed_options in recipe/nginx.rb
    def computed_options
    [
      '--prefix=/',
      '--error-log-path=stderr',
      '--with-http_ssl_module',
      '--with-http_v2_module',
      '--with-http_realip_module',
      '--with-http_gunzip_module',
      '--with-http_gzip_static_module',
      '--with-http_auth_request_module',
      '--with-http_random_index_module',
      '--with-http_secure_link_module',
      '--with-http_stub_status_module',
      '--without-http_uwsgi_module',
      '--without-http_scgi_module',
      '--with-pcre',
      '--with-pcre-jit',
      '--without-pcre2',
      '--with-cc-opt=-fPIC -pie',
      '--with-ld-opt=-fPIC -pie -z now',
      '--with-compat',
      '--with-stream=dynamic',
      '--with-http_sub_module',
      '--add-dynamic-module=/binary-builder/njs/nginx',
    ]
  end
5. Run cloudfoundry/cflinuxfs in it mode
    _docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs3 bash _
6. Update nginx version
    _version=1.23.3
    gpg_signature_url="http://nginx.org/download/nginx-${version}.tar.gz.asc"
    gpg_signature=`curl -sL ${gpg_signature_url}`_
7. Build
    _./bin/binary-builder --name=nginx-static --gpg-rsa-key-id=A1C052F8 --version=$version --gpg-signature="${gpg_signature}"_
8. Get ngx_http_js_module.so from objs folder

Refences:
1. http://nginx.org/en/docs/njs/install.html
2. http://nginx.org/en/docs/configure.html
3. https://github.com/cloudfoundry/binary-builder
4. https://github.com/nginx/njs/tree/0.7.8
5. http://nginx.org/download/
6. https://jcartw.medium.com/how-to-install-nginx-from-source-with-njs-javascript-module-443e8e3a0cf2
