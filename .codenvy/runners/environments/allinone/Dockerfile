FROM codenvy/angular-yeoman

RUN sudo -E bash -c "apt-get -y --no-install-recommends install mysql-server" && \
    sudo sed -i.bak 's/127.0.0.1/0.0.0.0/g' /etc/mysql/my.cnf && \
    sudo service mysql restart && \
    sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'; FLUSH PRIVILEGES;"
 
EXPOSE 3306
 
# Create MySQL user and database, and start it
RUN sudo service mysql restart && \
    CODENVY_MYSQL_PASSWORD=password && \
    CODENVY_MYSQL_DB=test && \
    CODENVY_MYSQL_USER=test && \
    echo "MySQL password: $CODENVY_MYSQL_PASSWORD" >> /home/user/.mysqlrc && \
    echo "MySQL user    : $CODENVY_MYSQL_USER" >> /home/user/.mysqlrc && \
    echo "MySQL Database:$CODENVY_MYSQL_DB" >> /home/user/.mysqlrc && \
    sudo mysql -uroot -e "CREATE USER '$CODENVY_MYSQL_USER'@'%' IDENTIFIED BY '"$CODENVY_MYSQL_PASSWORD"'" && \
    sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO '$CODENVY_MYSQL_USER'@'%' IDENTIFIED BY '"$CODENVY_MYSQL_PASSWORD"'; FLUSH PRIVILEGES;" && \
    sudo mysql -uroot -e "CREATE DATABASE $CODENVY_MYSQL_DB;"

VOLUME ["/home/user/application"]
ENV CODENVY_APP_BIND_DIR /home/user/application

ADD $src$/package.json /tmp/application/package.json

RUN cd /tmp/application && npm install

# ADD $src$ /home/user/application/

EXPOSE 8080

ENV CODENVY_APP_PORT_8080_HTTP 8080

WORKDIR /home/user/application

# 1. Update permissions
# 2. Copy nodejs modules to the application folder
# 3. Update permissions recursively
# 4. Makes newly created files accessible for anyone
# 5. Start application
CMD sudo service mysql start > /dev/null && \
sudo chmod a+rw /home/user/application/ && \
cp -a /tmp/application/node_modules /home/user/application/ && \
sudo chmod a+rw -R /home/user/application/ && \
umask 0 && \
npm start && sleep 356d
