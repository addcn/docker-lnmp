#
# Dockerfile for building Nginx images
# 
# https://github.com/addcn/docker-lnmp.git
#

FROM centos:centos7
MAINTAINER dodo <lhuibo@gmail.com>

ENV TZ "Asia/Shanghai"

#Yum
RUN yum -y update && \
    yum install -y gcc automake autoconf libtool make gcc-c++ vixie-cron  wget zlib  file openssl-devel sharutils zip  bash vim cyrus-sasl-devel libmemcached libmemcached-devel libyaml libyaml-devel unzip libvpx-devel openssl-devel ImageMagick-devel  autoconf  tar gcc libxml2-devel gd-devel libmcrypt-devel libmcrypt mcrypt mhash libmcrypt libmcrypt-devel libxml2 libxml2-devel bzip2 bzip2-devel curl curl-devel libjpeg libjpeg-devel libpng libpng-devel freetype-devel bison libtool-ltdl-devel net-tools && \
    yum clean all

#Nginx
RUN cd /tmp && \
  wget http://nginx.org/download/nginx-1.11.5.tar.gz && \
  tar xzf nginx-1.11.5.tar.gz && \
  cd /tmp/nginx-1.11.5 && \
  ./configure \
    --prefix=/usr/local/nginx \
    --with-http_ssl_module --with-http_sub_module --with-http_dav_module --with-http_flv_module \
    --with-http_gzip_static_module --with-http_stub_status_module --with-debug && \
    make && \
    make install

#配置nginx
ENV HTTP_PHP_CONFIG \\\n\\\t#php\\\n\\\tlocation ~ \\\\.php$ {\\\n\\\t\\\troot    html;\\\n\\\t\\\tfastcgi_pass   php7:9000;\\\n\\\t\\\tfastcgi_index    index.php;\\\n\\\t\\\tfastcgi_param  SCRIPT_FILENAME    /usr/local/nginx/html\$fastcgi_script_name;\\\n\\\t\\\tinclude    fastcgi_params;\\\n\\\t}\\\n\\\n\\\t

RUN sed -i -e "s@# deny access to .htaccess files, if Apache@${HTTP_PHP_CONFIG}# deny access to .htaccess files, if Apache@" /usr/local/nginx/conf/nginx.conf


EXPOSE 80 443

#启动nginx
ENTRYPOINT ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]
