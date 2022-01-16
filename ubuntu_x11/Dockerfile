from ubuntu
MAINTAINER "Bruce Allen"

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get install -y build-essential libc6-dev-i386 csh
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y python3-pyqt5
RUN DEBIAN_FRONTEND=noninteractive apt-get install python3-pyqt5.qtsvg
RUN apt-get update
RUN apt-get install -y python3-pip
RUN pip3 install --upgrade pip

# Qt requires non-root
RUN adduser --quiet --disabled-password mp_user && usermod -a -G audio mp_user
USER mp_user:audio
RUN pip3 install qdarkstyle --no-warn-script-location

# WORKDIR does not create directories
RUN mkdir /home/mp_user/mp_gryphon
RUN mkdir /home/mp_user/mp_gryphon/trace-generator
RUN mkdir /home/mp_user/mp_gryphon/gryphon
RUN mkdir /home/mp_user/mp_gryphon/preloaded-examples
WORKDIR /home/mp_user/mp_gryphon
ADD --chown=mp_user:audio gryphon.tar gryphon
ADD --chown=mp_user:audio trace-generator.tar trace-generator
ADD --chown=mp_user:audio preloaded-examples.tar preloaded-examples

# ADD chown doesn't work
USER root
run chown -R mp_user .
USER mp_user:audio

# build trace-generator
WORKDIR /home/mp_user/mp_gryphon/trace-generator/RIGAL/rigsc.446/src
RUN make

# Qt environment
ENV QT_DEBUG_PLUGINS=1

# open into Gryphon
WORKDIR /home/mp_user/mp_gryphon/gryphon/python
ENTRYPOINT ["python3", "mp.py", "-v"]

