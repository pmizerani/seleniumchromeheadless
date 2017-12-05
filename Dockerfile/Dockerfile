# Grab Ubuntu
FROM ubuntu:latest

# Install utilities
RUN apt-get update --fix-missing && apt-get install -y sudo wget curl git python unzip xvfb build-essential

# Install node 6
RUN curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash - &&\
sudo apt-get install -y nodejs

# Install Chrome for Ubuntu
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - &&\
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list' &&\
sudo apt-get update &&\
sudo apt-get install -y google-chrome-stable

# Install ChromeDriver
RUN wget https://chromedriver.storage.googleapis.com/2.33/chromedriver_linux64.zip &&\
unzip chromedriver_linux64.zip && chmod +x chromedriver &&\
mv -f chromedriver /usr/local/share/chromedriver &&\
ln -s /usr/local/share/chromedriver /usr/local/bin/chromedriver &&\
ln -s /usr/local/share/chromedriver /usr/bin/chromedriver

# Fix for the issue with Selenium, as described here:
# https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null SCREEN_RES=1280x1024x24
