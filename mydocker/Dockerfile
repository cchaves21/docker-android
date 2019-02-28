
FROM cchaves21/appium-base:latest



WORKDIR /root

#===============
# Set JAVA_HOME
#===============

# set JAVA_HOME
ENV JAVA_HOME /usr/lib/jvm/java-openjdk


ENV PATH=$PATH:$HOME/bin:$JAVA_HOME/bin

#=====================
# Install Android SDK
#=====================
ARG SDK_VERSION=sdk-tools-linux-4333796
ARG ANDROID_BUILD_TOOLS_VERSION=26.0.0
ARG ANDROID_PLATFORM_VERSION="android-26"

ENV SDK_VERSION=$SDK_VERSION \
ANDROID_BUILD_TOOLS_VERSION=$ANDROID_BUILD_TOOLS_VERSION \
ANDROID_HOME=/root

RUN unzip tools.zip && rm tools.zip && \
chmod a+x -R $ANDROID_HOME && \
chown -R root:root $ANDROID_HOME

COPY Devices skins ./.android/
COPY Testes ./

ENV PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin

RUN mkdir -p ~/.android && \
touch ~/.android/repositories.cfg && \
echo y | sdkmanager "platform-tools" && \
echo y | sdkmanager "build-tools;$ANDROID_BUILD_TOOLS_VERSION" && \
echo y | sdkmanager "platforms;$ANDROID_PLATFORM_VERSION"

ENV PATH=$PATH:$ANDROID_HOME/platform-tools:$ANDROID_HOME/build-tools

ADD license_accepter.sh /root/
RUN /root/license_accepter.sh $ANDROID_HOME

RUN $ANDROID_HOME/tools/bin/sdkmanager emulator
RUN $ANDROID_HOME/tools/bin/sdkmanager "system-images;android-26;google_apis;x86"
EXPOSE 5037 5554 5555 5900
#====================================
# Install latest nodejs, npm, appium
# Using this workaround to install Appium ->   -> Please remove this workaround asap
#====================================

ARG APPIUM_VERSION=1.10.0
ENV APPIUM_VERSION=$APPIUM_VERSION

RUN npm install -g appium@${APPIUM_VERSION} --unsafe-perm=true --allow-root && \
exit 0 && \
npm cache clean && \
dnf remove npm && \
dnf clean


EXPOSE 4723

WORKDIR ./qa-android/

#COPY Gemfile Gemfile.lock ./

RUN bundle install

RUN appium -v
RUN ruby -v

WORKDIR /root

CMD /usr/bin/appium

