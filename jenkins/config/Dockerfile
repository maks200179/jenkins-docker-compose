FROM maxfr1158/centos7:31.19

RUN localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.UTF-8
ENV container docker

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;


# Install initscripts openssh-server openssh-clients	
RUN yum -y install  openssh-server openssh-clients

# Set root password
RUN echo 'root:exemple' | chpasswd


RUN  rm -rf /var/cache/yum
RUN  yum -y update
RUN  yum install -y initscripts


#java
RUN  yum -y install java-1.8.0-openjdk-devel


#open-ssl
RUN  yum -y install openssl
RUN  mkdir /etc/ssl/private && chmod 700 /etc/ssl/private 
RUN  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/jenkins-selfsigned.key -out /etc/ssl/certs/jenkins-selfsigned.crt  -subj "/C=IL/ST=Merkaz/L=Revadim/O=Global Security/OU=IT Department/CN=jenkins.smart-farm.co" 

#install GIT
RUN yum -y install git


#install jenkins
RUN  yum -y install wget
RUN  wget -O /etc/yum.repos.d/jenkins.repo  https://pkg.jenkins.io/redhat/jenkins.repo
RUN  rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
RUN  yum -y install jenkins


#clean cash
RUN yum clean all
RUN rm -rf /var/cache/yum


# Set sshd config
RUN rm /etc/ssh/sshd_config
COPY sshd_config /etc/ssh/
RUN chown root:root /etc/ssh/sshd_config
RUN chmod -R 600 /etc/ssh/sshd_config


# Setting DateTime Zone
RUN cp -p /usr/share/zoneinfo/Asia/Jerusalem /etc/localtime


#Fix [FAILED] Failed to start LSB: Bring up/down networking.
RUN touch /etc/sysconfig/network 


#jenkins timezone
RUN echo "JENKINS_JAVA_OPTIONS="-Dorg.apache.commons.jelly.tags.fmt.timeZone=Asia/Jerusalem"" >> /etc/sysconfig/jenkins


ENTRYPOINT ["/usr/sbin/init"]
VOLUME [ "/sys/fs/cgroup" ]


