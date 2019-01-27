FROM ubuntu:16.04

ENV USER=player

# Create user
RUN groupadd --gid 1000 $USER && \
  useradd --uid 1000 --gid $USER -G video --create-home $USER && \
  chown -R $USER:$USER /home/$USER

# Install dependencies
RUN apt-get update && apt-get upgrade -y && \
  apt-get install -y software-properties-common libgl1-mesa-glx libgl1-mesa-dri && \
  rm -rf /var/lib/apt/lists/*

# Install youtube-dl
RUN add-apt-repository -y ppa:nilarimogard/webupd8 && apt-get update && \
  apt-get install -y youtube-dl && \
  rm -rf /var/lib/apt/lists/*

# Install mpv
RUN add-apt-repository -y ppa:mc3man/mpv-tests && apt-get update && \
  apt-get install -y mpv libmpv-dev && \
  rm -rf /var/lib/apt/lists/*

# Configure MPV
ADD mpv.conf /home/$USER/.config/mpv/mpv.conf

# Start player
USER $USER
ENV DISPLAY :0
CMD ["mpv", "/videos"]
