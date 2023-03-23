http://nginx.org/en/docs/njs/install.html
http://nginx.org/en/docs/configure.html
https://github.com/cloudfoundry/binary-builder
https://github.com/nginx/njs/tree/0.7.8
http://nginx.org/download/
https://jcartw.medium.com/how-to-install-nginx-from-source-with-njs-javascript-module-443e8e3a0cf2

docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs3:latest bash
docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs3:0.333.0 bash
version=1.23.3
gpg_signature_url="http://nginx.org/download/nginx-${version}.tar.gz.asc"
gpg_signature=`curl -sL ${gpg_signature_url}`
./bin/binary-builder --name=nginx-static --gpg-rsa-key-id=A1C052F8 --version=$version --gpg-signature="${gpg_signature}"

or 

docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs3 ./bin/binary-builder --name=nginx-static --gpg-rsa-key-id=A1C052F8 --version=$version --gpg-signature="${gpg_signature}"


./configure --sbin-path=/usr/local/nginx/nginx --conf-path=/usr/local/nginx/nginx.conf --pid-path=/usr/local/nginx/nginx.pid --error-log-path=stderr --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_auth_request_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --without-http_uwsgi_module --without-http_scgi_module --with-pcre --with-pcre-jit --without-pcre2 --with-cc-opt='-fPIC -pie' --with-ld-opt='-fPIC -pie -z now' --with-compat --with-stream=dynamic --with-http_sub_module --add-dynamic-module='/binary-builder/njs/nginx'
