RUN mkdir /src && \
    mkdir /app && \
    mkdir /exec && \
    echo "\n*** Updating system ***\n" && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - > /dev/null && \
    echo "deb http://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list > /dev/null && \
    apt-get update -qq -y && \
    apt-get dist-upgrade -qq -y && \
    apt-get install -qq -y wget git unzip && \
    apt-get autoremove -qq -y && \
    apt-get clean -qq && \
    echo "\n*** nvm ***\n" && \
    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash && \
    echo "\n*** yarn ***\n" && \
    apt-get install -qq -y yarn && \
    echo "\n*** composer ***\n" && \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
# Changes automatically, we can not use this
# https://getcomposer.org/download/
#    php -r "if (hash_file('SHA384', 'composer-setup.php') === '070854512ef404f16bac87071a6db9fd9721da1684cd4589b1196c3faf71b9a2682e2311b36a5079825e155ac7ce150d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    echo "\n*** Cleanup ***\n" && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY scripts/build.sh /exec/build.sh
COPY scripts/composer /usr/local/bin/composer

ONBUILD COPY prebuild.sh /exec/prebuild.sh
ONBUILD COPY postbuild.sh /exec/postbuild.sh

RUN chmod 700 /exec/build.sh

ENTRYPOINT /exec/build.sh
