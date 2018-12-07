# mail-proxy
#############################git Code########################################

cd /top

git clone git@10.121.8.131:guozh10/mail-proxy.git

##############################pcre   install ################################

tar zxvf pcre-8.35.tar.gz -C /tmp

yum -y install gcc*

cd /tmp/pcre-8.35/

./configure --prefix=/usr/local/pcre

make && make install

tar zxvf openssl-1.0.2g.tar.gz -C /tmp/

####################################openssl install ##########################

cd /tmp/openssl-1.0.2g/

./config enable-tl***t

make && make install

mv -f /usr/bin/openssl /usr/bin/openssl.old

mv -f /usr/include/openssl /usr/include/openssl.old


ln -sf /usr/local/ssl/bin/openssl /usr/bin/openssl

ln -sf /usr/local/ssl/include/openssl /usr/include/openssl

############################# nginx install #################################


yum -y install patch zlib zlib-devel

tar zxf nginx-1.13.4.tar.gz

cd nginx-1.13.4/

patch -p1 < ../nginx_upstream_check_module/check_1.12.1+.patch

./configure --user=www --group=www \

--prefix=/usr/local/nginx --with-http_stub_status_module \

--with-stream=dynamic --with-stream_ssl_module \

--with-pcre=/tmp/pcre-8.35 --with-http_ssl_module \

--with-openssl=/tmp/openssl-1.0.2g \

--add-module=../nginx_upstream_check_module


make && make install

userad www

#################################copy  .conf file ##########################

cd /opt/mail-proxy

cp -rf conf/ /usr/local/nginx/conf

##########################################check nginx service ##############

cd /usr/local/nginx
./sbin/nginx -t

#####################################start nginx############################

./sbin/nginx







