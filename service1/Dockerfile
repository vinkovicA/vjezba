FROM python:3.8-alpine

RUN apk update; apk upgrade; apk add curl
RUN curl -sL https://github.com/openfaas/faas/releases/download/0.9.14/fwatchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog

ENV fprocess="python3 entrypoint.py"
COPY entrypoint.py /

EXPOSE 9000

CMD ["fwatchdog", "${fprocess}"]