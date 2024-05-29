FROM tomcat:9.0-slim
USER root
WORKDIR /opt/
ENV APP_HOME=/usr/local/tomcat
ENV PORT=8080
# Install application $studentwar from s3 bucket
ADD https://s3.ap-south-1.amazonaws.com/host-student.war/student.war $APP_HOME/webapps/
# Install mysql-connector to communicate backend and database
ADD https://s3.ap-south-1.amazonaws.com/host-student.war/mysql-connector.jar $APP_HOME/lib/
#COPY cofig to context.xml
COPY config /opt
RUN sed -i '20r /opt/config' /usr/local/tomcat/conf/context.xml
EXPOSE $PORT
CMD ["catalina.sh", "run"]
