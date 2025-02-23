FROM --platform=linux/amd64 debian:stable
    
ARG ANKI_VERSION=latest

# Install dependencies https://github.com/ankitects/anki/blob/main/docs/docker/Dockerfile
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    jq \
    libasound2 \
    libdbus-1-3 \
    libfontconfig1 \
    libfreetype6 \
    libgl1 \
    libegl1 \
    libglib2.0-0 \
    libnss3 \
    libxcb-cursor0 \
    libxcb-icccm4 \
    libxcb-image0 \
    libxcb-keysyms1 \
    libxcb-randr0 \
    libxcb-render-util0 \
    libxcb-shape0 \
    libxcb-xinerama0 \
    libxcb-xkb1 \
    libxcomposite1 \
    libxcursor1 \
    libxi6 \
    libxkbcommon0 \
    libxkbcommon-x11-0 \
    libxrandr2 \
    libxrender1 \
    libxtst6 \
    libxdamage1 \
    libxkbfile1 \
    ssh \
    wget \
    unzip \
    xdg-utils \
    zstd \
  && rm -rf /var/lib/apt/lists/*

# Set the locale for Anki
RUN apt-get update && apt-get install -y locales && \
    sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen en_US.UTF-8

# Download specified version of Anki
RUN if [ "$ANKI_VERSION" = "latest" ]; then \
    ANKI_VERSION=$(wget -qO- https://api.github.com/repos/ankitects/anki/releases/latest | jq -r '.tag_name'); \
  fi \
  && echo "Using anki version: $ANKI_VERSION" \
  && wget -O /tmp/anki.tar.zst $(wget -qO- https://api.github.com/repos/ankitects/anki/releases/tags/$ANKI_VERSION \
    | jq -r '.assets[] | select(.name | test("linux-qt6\\.tar\\.zst$")) | .browser_download_url') \
  && mkdir -p /tmp/anki \
  && tar --use-compress-program=unzstd -xf /tmp/anki.tar.zst --strip-components=1 -C /tmp/anki \
  && rm /tmp/anki.tar.zst

# Install Anki
RUN cd /tmp/anki && ./install.sh || exit 0 && cd - && rm -rf /tmp/anki