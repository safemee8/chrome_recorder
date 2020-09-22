FROM debian:stable

## For chromedriver installation: curl/wget/libgconf/unzip
RUN apt-get update -y && apt-get install -y wget curl unzip libgconf-2-4
## For project usage: python3/python3-pip/chromium/xvfb
RUN apt-get update -y && apt-get install -y chromium xvfb python3 python3-pip


# Download, unzip, and install chromedriver
RUN wget -O /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip chromedriver -d /usr/local/bin/

# install ffmpeg and libraries
RUN apt-get install -y software-properties-common && add-apt-repository ppa:jonathonf/ffmpeg-4 && apt-get -y update && apt-get -y install firefox python3-pip x264 xvfb ffmpeg pulseaudio nasm libx264-dev libx265-dev libnuma-dev libvpx-dev libfdk-aac-dev libmp3lame-dev libopus-dev

# Create directory for project name (ensure it does not conflict with default debian /opt/ directories).
RUN mkdir -p /recorder
WORKDIR /recorder

## Your python project dependencies
RUN pip3 install geckodriver-autoinstaller selenium

## Copy over project/script (feel free to combine these if your project is a combination of both directories and top-level files)
### For projects which are modules
COPY . .
## or install from dependencies.txt, comment above and uncomment below
RUN pip3 install -r requirements.txt

# Set display port and dbus env to avoid hanging
ENV DISPLAY=:99
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null


# Bash script to invoke xvfb, any preliminary commands, then invoke project
COPY run.sh .
CMD /bin/bash run.sh