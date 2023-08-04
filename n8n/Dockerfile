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
    then apk --no-cache --update add libreoffice ; \
    fi
USER node
