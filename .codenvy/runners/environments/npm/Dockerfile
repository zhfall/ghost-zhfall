FROM codenvy/angular-yeoman

ADD $src$/package.json /tmp/application/package.json

RUN cd /tmp/application && npm install && npm install sqlite3

ADD $src$ /home/user/application/

EXPOSE 8080

ENV CODENVY_APP_PORT_8080_HTTP 8080

# 1. Update permissions
# 2. Copy nodejs modules to the application folder
# 3. Update permissions recursively
# 4. Makes newly created files accessible for anyone
# 5. Start application
CMD sudo chmod a+rw /home/user/application/ && \
cp -a /tmp/application/node_modules /home/user/application/ && \
sudo chmod a+rw -R /home/user/application/ && \
umask 0 && \
npm start
