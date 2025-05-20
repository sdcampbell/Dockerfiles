FROM kalilinux/kali-rolling

RUN apt update -y && \
    apt install autopsy socat -y

RUN mkdir /images

RUN echo "autopsy & socat TCP-LISTEN:9998,fork TCP:127.0.0.1:9999" > /usr/bin/startup.sh

CMD [ "bash", "/usr/bin/startup.sh" ]
