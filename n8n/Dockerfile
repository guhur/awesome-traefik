FROM n8nio/n8n:latest

# Install additional packages
RUN apk add --update --no-cache python3 build-base && ln -sf python3 /usr/bin/python
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade pip setuptools pypdf
# RUN apk add --update  pkgconfig
# RUN apk add --update  build-base
# RUN apk add --update  gcc
# RUN pip3 install pymupdf
