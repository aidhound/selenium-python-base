FROM python:3.10.11

ENV CONTEXT .

RUN apt-get update && apt-get install -yq \
    firefox-esr \
    chromium=112.0.5615.49-2~deb11u2 \
    git-core \
    xvfb \
    xsel \
    unzip \
    gconf-gsettings-backend \
    python3-setuptools \
    libncurses5 \
    libxml2-dev \
    libxslt-dev \
    libz-dev \
    xclip

# GeckoDriver v0.24.0
RUN wget -q "https://github.com/mozilla/geckodriver/releases/download/v0.24.0/geckodriver-v0.24.0-linux64.tar.gz" -O /tmp/geckodriver.tgz \
    && tar zxf /tmp/geckodriver.tgz -C /usr/bin/ \
    && rm /tmp/geckodriver.tgz

# chromeDriver v112.0.5615.49
RUN wget -q "https://chromedriver.storage.googleapis.com/112.0.5615.49/chromedriver_linux64.zip" -O /tmp/chromedriver.zip \
    && unzip /tmp/chromedriver.zip -d /usr/bin/ \
    && rm /tmp/chromedriver.zip

# xvfb - X server display
#ADD selenium-base-image/xvfb-chromium /usr/bin/xvfb-chromium
#RUN ln -s /usr/bin/xvfb-chromium /usr/bin/google-chrome \
#    && chmod 777 /usr/bin/xvfb-chromium

# create symlinks to chromedriver and geckodriver (to the PATH)
RUN ln -s /usr/bin/geckodriver /usr/bin/chromium-browser \
    && chmod 777 /usr/bin/geckodriver \
    && chmod 777 /usr/bin/chromium-browser

# pip
COPY ${CONTEXT}/requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt
#RUN easy_install -U setuptools
RUN pip install pytest-dependency

#CMD ["/bin/true"]

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
