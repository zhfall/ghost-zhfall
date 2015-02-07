FROM codenvy/angular-yeoman

ADD $src$ /home/user/application/

WORKDIR /home/user/application

RUN npm install && npm install sqlite3

EXPOSE 8080

ENV CODENVY_APP_PORT_8080_HTTP 8080

CMD sudo chmod a+rw -R /home/user/application/ && \
umask 0 && \
npm start
