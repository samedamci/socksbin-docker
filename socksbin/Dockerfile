FROM python:3.8-alpine

LABEL maintainer="samedamci@disroot.org"
LABEL version="0.1"
LABEL description="SocksBin simple pastebin service in Docker."

RUN apk add --update --no-cache git && \
	git clone https://github.com/MagnumDingusEdu/SocksBin.git && \
	cd SocksBin && \
	pip install -r requirements.txt && \
	chmod +x paster.py

VOLUME /pastes

ENV DOMAIN localhost

COPY start.sh /start.sh

RUN chmod +x /start.sh
CMD ["./start.sh"]

EXPOSE 8888
