FROM ubuntu

RUN apt update
RUN apt upgrade -y
RUN apt install -y\
    openjdk-8-jre-headless\
    wget

WORKDIR /opt/android-sdk/
COPY cmdline-tools.tar .
RUN tar -xpf cmdline-tools.tar
RUN rm cmdline-tools.tar

RUN ln -s /opt/android-sdk/cmdline-tools/tools/bin/sdkmanager /usr/local/bin/
RUN yes | sdkmanager tools platform-tools $(sdkmanager --list | grep build-tools | tail -n1 | cut -d" " -f3)
RUN chmod -R +x /opt/android-sdk/
RUN ln -fs /opt/android-sdk/platform-tools/adb /usr/local/bin/
RUN ln -fs /opt/android-sdk/platform-tools/fastboot /usr/local/bin/
RUN ln -fs /opt/android-sdk/build-tools/$(ls -1 /opt/android-sdk/build-tools/ | tail -n1)/aapt /usr/local/bin/

WORKDIR /etc/udev/rules.d/
#COPY 51-android.rules .
RUN wget -O /etc/udev/rules.d/51-android.rules https://raw.githubusercontent.com/snowdream/51-android/master/51-android.rules
RUN chmod a+r 51-android.rules

WORKDIR /root
COPY update-android-sdk_linux.sh .
RUN echo "alias adb='adb -H host.docker.internal'" >> .bash_aliases