FROM n8nio/n8n:next
ARG WITH_PYPDF=false
ARG WITH_LATEX=false
ARG WITH_DOC2PDF=false

# Install additional packages depending on build arguments
USER root
RUN if [ "$WITH_PYPDF" = "true" ]; \
    then apk add --update --no-cache python3 build-base gcc python3-dev && \
         ln -sf python3 /usr/bin/python && \
	 python3 -m ensurepip && \
         pip3 install --no-cache --upgrade pip setuptools typed-argument-parser poetry pypdf; \
    fi
RUN if [ "$WITH_LATEX" = "true" ]; \
    then apk --no-cache --update add texmf-dist texlive-xetex texlive; \
    fi
RUN if [ "$WITH_DOC2PDF" = "true" ]; \
    then apk --no-cache --update add libreoffice && \
        wget https://github.com/google/fonts/archive/main.tar.gz -O gf.tar.gz --no-check-certificate && \
        tar -xf gf.tar.gz && \
        mkdir -p /usr/share/fonts/truetype/google-fonts && \
        find $PWD/fonts-main/ -name "*.ttf" -exec install -m644 {} /usr/share/fonts/truetype/google-fonts/ \; || return 1 && \
        rm -f gf.tar.gz && \
        fc-cache -f && rm -rf /var/cache/*; \
    fi
USER node
