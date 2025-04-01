FROM ubuntu:latest

# მიღება devcontainer.json-იდან
ARG LOCAL_WORKSPACE_FOLDER_BASENAME

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apache2 \
    mysql-server \
    php \
    libapache2-mod-php \
    php-mysql \
    php-zip \
    php-gd \
    php-json \
    php-mbstring \
    php-curl \
    php-xml \
    phpmyadmin \
    curl \
    git \
    vim \
    unzip \
    nano

# WP-CLI ინსტალაცია
RUN curl -OL https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
    chmod +x wp-cli.phar && \
    mv wp-cli.phar /usr/local/bin/wp

# MySQL-ის bind-address-ის გასწორება, რომ сыртიდან იყოს მისაწვდომი
RUN sed -i 's/bind-address.*/bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf && \
    sed -i 's/mysqlx-bind-address.*/mysqlx-bind-address = 0.0.0.0/' /etc/mysql/mysql.conf.d/mysqld.cnf

# Apache მოდულების ჩართვა + ServerName
RUN a2enmod rewrite && \
    a2enmod headers && \
    a2enmod ssl && \
    echo "ServerName localhost" >> /etc/apache2/apache2.conf

# ვქმნით (ან ვაკონფიგურირებთ) საქაღალდეს WordPress-ისთვის
RUN mkdir -p /workspaces/$LOCAL_WORKSPACE_FOLDER_BASENAME/wordpress && \
    chown -R www-data:www-data /workspaces/$LOCAL_WORKSPACE_FOLDER_BASENAME/wordpress && \
    chmod -R 755 /workspaces/$LOCAL_WORKSPACE_FOLDER_BASENAME/wordpress

# ვცვლით DocumentRoot-ს /var/www/html-დან ჩვენს wordpress საქაღალდედ
RUN sed -i "s#DocumentRoot /var/www/html#DocumentRoot /workspaces/$LOCAL_WORKSPACE_FOLDER_BASENAME/wordpress#g" /etc/apache2/sites-available/000-default.conf && \
    echo '<Directory /workspaces/'$LOCAL_WORKSPACE_FOLDER_BASENAME'/wordpress>' >> /etc/apache2/sites-available/000-default.conf && \
    echo '    Options FollowSymLinks' >> /etc/apache2/sites-available/000-default.conf && \
    echo '    AllowOverride All' >> /etc/apache2/sites-available/000-default.conf && \
    echo '    Require all granted' >> /etc/apache2/sites-available/000-default.conf && \
    echo '</Directory>' >> /etc/apache2/sites-available/000-default.conf

# PhpMyAdmin-ის საკონფიგურაციო ფაილები
RUN echo "<?php" > /etc/phpmyadmin/config.inc.php && \
    echo "\$cfg['blowfish_secret'] = 'a8b7c6d5e4f3g2h1i0j9k8l7m6n5o4p3';" >> /etc/phpmyadmin/config.inc.php && \
    echo "\$cfg['TempDir'] = '/var/lib/phpmyadmin/tmp';" >> /etc/phpmyadmin/config.inc.php && \
    echo "\$cfg['Servers'][1]['auth_type'] = 'cookie';" >> /etc/phpmyadmin/config.inc.php && \
    echo "\$cfg['Servers'][1]['host'] = '127.0.0.1';" >> /etc/phpmyadmin/config.inc.php

# PhpMyAdmin-ის ვირტუალური მასპინძლის (VirtualHost) დამატება 8080 პორტზე
RUN echo "Listen 8080" >> /etc/apache2/ports.conf && \
    echo '<VirtualHost *:8080>' > /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '    DocumentRoot /usr/share/phpmyadmin' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '    <Directory /usr/share/phpmyadmin>' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '        Options FollowSymLinks' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '        DirectoryIndex index.php' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '        AllowOverride All' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '        Require all granted' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '    </Directory>' >> /etc/apache2/sites-available/phpmyadmin.conf && \
    echo '</VirtualHost>' >> /etc/apache2/sites-available/phpmyadmin.conf

# ნებართვები phpmyadmin-ისთვის
RUN chown -R www-data:www-data /usr/share/phpmyadmin && \
    chmod -R 755 /usr/share/phpmyadmin && \
    ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin && \
    a2ensite phpmyadmin.conf

# ვხსნით 80 და 8080 პორტებს
EXPOSE 80 8080

# Default CMD – MySQL და Apache2-ის გაშვება
CMD ["/bin/bash", "-c", "service mysql start && service apache2 start && tail -f /dev/null"]
