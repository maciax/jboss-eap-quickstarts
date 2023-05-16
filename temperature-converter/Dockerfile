#Dockerfile for EAP 8.0
FROM ubi8/ubi:8.7

MAINTAINER Alberto Macias amaciasr@redhat.com

ENV JBOSS_HOME /opt/rh/jboss-eap-8.0



RUN echo '%wheel ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
sed -i 's/.*requiretty$/Defaults !requiretty/' /etc/sudoers

#add a user for the application, with sudo permissions

RUN useradd jboss; echo jboss: | chpasswd; usermod -aG wheel jboss
#RUN usermod -aG wheel jboss


### comandos previos Actualizaciones

RUN yum update -y && \
yum install java-17-openjdk-devel -y
#&& \ 
#yum clean all -y


###manejar el archivo .zip
#create workdir

RUN mkdir -p /opt/rh

WORKDIR /opt/rh

ADD jboss-eap-8.0.zip /opt/rh/

RUN yum install unzip -y


RUN unzip jboss-eap-8.0.zip


#Crear un usuario Jboss

RUN $JBOSS_HOME/bin/add-user.sh admin admin@2021 --silent

#Agregar Java Web Application en la carpeta de despliegues de Jboss
ADD temperature-converter.war /opt/rh/jboss-eap-8.0/standalone/deployments

#set permission folder
RUN chown -R jboss:jboss /opt/rh

#Exponer puertos
EXPOSE 8080 9990 9999

USER jboss
ENTRYPOINT ["/opt/rh/jboss-eap-8.0/bin/standalone.sh"]

CMD ["-b", "0.0.0.0"]
#CMD ["-D", "FOREGROUND"]

#Construir la imagen 
#podman build --tag rhel/jboss -f ./Dockerfile  

#Correr el contenedor 
#podman run -dt -p 30080:8080 rhel/jboss 

#Validar despliegue 
#curl -s http://0.0.0.0:30080/temperature-converter/temperatureconvert.jsf 
