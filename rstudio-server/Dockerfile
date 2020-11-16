FROM ubuntu:bionic
LABEL org.label-schema.vendor="Centraal Bureau voor de Statistiek" \
      maintainer="Sander Janssen / Remco Vergoossen <sander.janssen@cbs.nl>"

### Add ENV / fix Timezone issue
ENV TZ=Europe/Amsterdam
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV DEBIAN_FRONTEND noninteractiv

### Install requirements
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
		software-properties-common \
                dirmngr \
                ed \
                less \
                locales \
                vim-tiny \
                wget \
                ca-certificates \
                gdebi-core \
                psmisc \
                libapparmor1 \
                libedit2 \
                sudo \
                lsb-release \
                libssl1.0.0 \
                libclang-dev \
                libc6 \
        && add-apt-repository --enable-source --yes "ppa:marutter/rrutter4.0" \
        && add-apt-repository --enable-source --yes "ppa:c2d4u.team/c2d4u4.0+"

### Configure UTF8 
RUN echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
	&& locale-gen en_US.utf8 \
	&& /usr/sbin/update-locale LANG=en_US.UTF-8

### Install R base package
RUN apt-get update \
        && apt-get install -y --no-install-recommends \
                 littler \
 		 r-base \
 		 r-base-dev \
 		 r-recommended \
  	&& ln -s /usr/lib/R/site-library/littler/examples/install.r /usr/local/bin/install.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/install2.r /usr/local/bin/install2.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/installGithub.r /usr/local/bin/installGithub.r \
 	&& ln -s /usr/lib/R/site-library/littler/examples/testInstalled.r /usr/local/bin/testInstalled.r \
 	&& install.r docopt \
 	&& rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
 	&& rm -rf /var/lib/apt/lists/*



### Download RStudio Server
RUN wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.3.1093-amd64.deb -P /tmp/
RUN gdebi -n /tmp/rstudio-server-1.3.1093-amd64.deb
ENV PATH=/usr/lib/rstudio-server/bin:$PATH

### Cleanup files
RUN rm -f /tmp/rstudio-server-1.3.1093-amd64.deb

### Test
ARG r_username
ARG r_password
RUN echo ${r_username}
RUN echo ${r_password}

### Add RStudio user
RUN useradd -m -d /home/${r_username} -s /bin/bash -p $(perl -e 'print crypt($ARGV[0], "password")' ${r_password}) ${r_username}
RUN chown ${r_username}:${r_username} /home/${r_username}
RUN usermod -a -G rstudio-server ${r_username}

### RStudio Server Config
#RUN echo "auth-required-user-group=rstudio-users" >> /etc/rstudio/rserver.conf
RUN echo "server-app-armor-enabled=0" >> /etc/rstudio/rserver.conf

### Generate startup script
RUN mkdir /rstudio-server
RUN touch /rstudio-server/startup.sh
RUN echo "#!/bin/bash" >> /rstudio-server/startup.sh
RUN echo "/etc/init.d/rstudio-server start" >> /rstudio-server/startup.sh
RUN echo "cat" >> /rstudio-server/startup.sh
RUN ["chmod", "+x", "/rstudio-server/startup.sh"]

# CMD /bin/bash
CMD ["/bin/bash","/rstudio-server/startup.sh"]