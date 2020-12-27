FROM centos:centos7

RUN yum -y install dnf curl wget vim \
python-setuptools hostname yum-utils which epel-release

RUN yum -y install httpd

RUN dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN dnf install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm
RUN yum install -y php80 php80-php-common php80-php-fpm php80-php-mysqlnd php80-php-devel \
php80-php-mbstring php80-php-gd php80-php-pdo php80-php-soap php80-php-pecl-xmlrpc php80-php-xml \
php80-php-bcmath php80-php-opcache php80-php-pecl-apcu php80-php-pecl-imagick php80-php-pecl-mcrypt 

RUN sed -i 's/AllowOverride None/AllowOverride All/g' /etc/httpd/conf/httpd.conf

RUN sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 60M/g' /etc/php.ini \
&& sed -i 's/post_max_size = 8M/post_max_size = 80M/g' /etc/php.ini \
&& sed -i 's/memory_limit = 128M/memory_limit = 512M/g' /etc/php.ini

# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY MariaDB.repo /etc/yum.repos.d/MariaDB.repo
RUN yum clean all;yum -y install mariadb-server mariadb-client
VOLUME /var/lib/mysql

# UTC Timezone & Networking
RUN ln -sf /usr/share/zoneinfo/UTC /etc/localtime \
	&& echo "NETWORKING=yes" > /etc/sysconfig/network

RUN yum -y install python-setuptools epel-release
RUN mkdir -p /var/log/supervisor && easy_install supervisor
COPY supervisord.conf /etc/supervisord.conf

EXPOSE 3306
EXPOSE 80
EXPOSE 443

CMD ["/usr/bin/supervisord"]
