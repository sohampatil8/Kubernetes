FROM tomcat:9.0-slim
WORKDIR /mnt
ENV my-home=/usr/local/tomcat
ENV port=8080
# Install application $studentwar from s3 bucket
ADD https://s3.ap-south-1.amazonaws.com/host-student.war/student.war $my-home/webapps
# Install mysql-connector to communicate backend and database
Add https://s3.ap-south-1.amazonaws.com/host-student.war/mysql-connector.jar $my-home/lib
#COPY cofig to context.xml
COPY config /mnt
RUN sed -i '20r /mnt/config' $my-home/conf/context.xml
EXPOSE $PORT
CMD ["catalina.sh", "run"]