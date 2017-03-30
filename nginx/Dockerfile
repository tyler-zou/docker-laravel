FROM daocloud.io/nginx

MAINTAINER Tyler <zzxaire@163.com>

#RUN usermod -u 1000 www-data

COPY nginx.conf /etc/nginx/

COPY site-enable/* /etc/nginx/sites-available/

ARG PHP_UPSTREAM=php-fpm

RUN unlink /etc/nginx/conf.d/default.conf && \ 
	echo "upstream php-upstream { server ${PHP_UPSTREAM}:9000; }" > /etc/nginx/conf.d/upstream.conf

#VOLUME ["/var/www"]

CMD ["nginx"]

EXPOSE 80 443
