FROM ubuntu:20.04

ENV ANDROID_HOME=/opt/android-sdk-linux \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    LANGUAGE=en_US:en

ENV ANDROID_PLATFORM_VERSION 29
ENV ANDROID_BUILD_TOOLS_VERSION 29.0.3

ENV ANDROID_SDK_ROOT=$ANDROID_HOME \
    PATH=${PATH}:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator

# Reference comes from https://developer.android.com/studio/#command-tools
ENV ANDROID_SDK_TOOLS_VERSION 6609375

RUN set -o xtrace \
    && cd /opt \
    && apt-get update \
    && apt-get install -y openjdk-8-jdk \
    && apt-get install -y sudo wget zip unzip git openssh-client curl bc software-properties-common build-essential ruby-full ruby-bundler lib32stdc++6 libstdc++6 libpulse0 libglu1-mesa locales lcov libsqlite3-0 --no-install-recommends \
    # for x86 emulators
    && apt-get install -y libxtst6 libnss3-dev libnspr4 libxss1 libasound2 libatk-bridge2.0-0 libgtk-3-0 libgdk-pixbuf2.0-0 \
    && rm -rf /var/lib/apt/lists/* \
    && sh -c 'echo "en_US.UTF-8 UTF-8" > /etc/locale.gen' \
    && locale-gen \
    && update-locale LANG=en_US.UTF-8 \
    && wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_TOOLS_VERSION}_latest.zip -O android-sdk-tools.zip \
    && mkdir -p ${ANDROID_HOME}/cmdline-tools/ \
    && unzip -q android-sdk-tools.zip -d ${ANDROID_HOME}/cmdline-tools/ \
    && rm android-sdk-tools.zip \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && yes | sdkmanager --licenses \
    && wget -O /usr/bin/android-wait-for-emulator https://raw.githubusercontent.com/travis-ci/travis-cookbooks/master/community-cookbooks/android-sdk/files/default/android-wait-for-emulator \
    && chmod +x /usr/bin/android-wait-for-emulator \
    && touch /root/.android/repositories.cfg \
    && sdkmanager platform-tools \
    && sdkmanager emulator \
    && mkdir -p /root/.android \
    && touch /root/.android/repositories.cfg \
    && yes | sdkmanager \
    "platforms;android-$ANDROID_PLATFORM_VERSION" \
    "build-tools;$ANDROID_BUILD_TOOLS_VERSION" \
    && rm -rf /var/lib/apt/lists/*

