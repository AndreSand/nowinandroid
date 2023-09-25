# ====================================================================== #
# Android SDK Docker Image
# ====================================================================== #

# Base image
# ---------------------------------------------------------------------- #
FROM ubuntu:22.04
#FROM ubuntu:latest

# Author
# ---------------------------------------------------------------------- #
# LABEL maintainer "Andres Sandoval"
LABEL authors="sandovalandres"

# support multiarch: i386 architecture
# install Java
# install essential tools
ARG JDK_VERSION=17
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc-s1 lib32ncurses6 lib32z1 zlib1g:i386 && \
    apt-get install -y --no-install-recommends openjdk-${JDK_VERSION}-jdk && \
    apt-get install -y --no-install-recommends git wget unzip && \
    apt-get clean && rm -rf /var/lib/apt/list/*

# download and install Kotlin compiler
# https://github.com/JetBrains/kotlin/releases/latest
ARG KOTLIN_VERSION=1.8.21
RUN cd /opt && \
    wget -q https://github.com/JetBrains/kotlin/releases/download/v${KOTLIN_VERSION}/kotlin-compiler-${KOTLIN_VERSION}.zip && \
    unzip *kotlin*.zip && \
    rm *kotlin*.zip

# download and install Android SDK
# https://developer.android.com/studio#command-tools
ARG ANDROID_SDK_VERSION=9477386
ENV ANDROID_HOME /opt/android-sdk
RUN mkdir -p ${ANDROID_HOME}/cmdline-tools && \
    wget -q https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_SDK_VERSION}_latest.zip && \
    unzip *tools*linux*.zip -d ${ANDROID_HOME}/cmdline-tools && \
    mv ${ANDROID_HOME}/cmdline-tools/cmdline-tools ${ANDROID_HOME}/cmdline-tools/tools && \
    rm *tools*linux*.zip

RUN echo "accepting licenses" && \
    echo -e 24333f8a63b6825ea9c5514f83c2829b004d1fee > ${ANDROID_HOME}/licenses && \
    echo -e 8933bad161af4178b1185d1a37fbf41ea5269c55 >> ${ANDROID_HOME}/licenses && \
    echo -e d56f5187479451eabf01fb78af6dfcb131a6481e >> ${ANDROID_HOME}/licenses && \
    echo -e 84831b9409646a918e30573bab4c9c91346d8abd >> ${ANDROID_HOME}/licenses && \
    echo -e 504667f4c0de7af1a06de9f4b1727b84351f2910 > ${ANDROID_HOME}/licenses

RUN echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager --licenses

RUN echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager "build-tools;34.0.0" && \
    echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager "build-tools;33.0.1" && \
    echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager "platforms;android-33" && \
    echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager "platforms;android-34" && \
    echo yes | ${ANDROID_HOME}/cmdline-tools/tools/bin/sdkmanager "platform-tools"


# set the environment variables
ENV JAVA_HOME /usr/lib/jvm/java-${JDK_VERSION}-openjdk-amd64
ENV KOTLIN_HOME /opt/kotlinc
ENV PATH ${PATH}:${KOTLIN_HOME}/bin:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/cmdline-tools/tools/bin:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/emulator

