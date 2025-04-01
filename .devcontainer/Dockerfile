FROM wordpress:latest

RUN apt-get update && apt-get install -y \
    curl \
    less \
    mariadb-client \
    unzip \
    vim \
    git

RUN curl -OL https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp
