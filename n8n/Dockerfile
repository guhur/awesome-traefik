FROM n8nio/n8n:next
ARG WITH_PYPDF=false
ARG WITH_LATEX=false
ARG WITH_DOC2PDF=false
ARG WITH_POSTGRES=false
ARG LOCALE

# Install additional packages depending on build arguments
USER root

# Install extra fonts & missing headers for Python packages
RUN apk --update --no-cache add ttf-dejavu font-noto-emoji fontconfig linux-headers build-base gcc python3 python3-dev  && rm -rf /var/cache/apk/*
COPY fonts/ /usr/share/fonts/truetype/
RUN fc-cache -fv

RUN if [ "$WITH_POSTGRES" = "true" ] ; \
     then apk --update --no-cache add postgresql-dev postgresql-client && \
     rm -rf /var/cache/apk/* ; \
     fi

RUN if [ "$WITH_PYPDF" = "true" ]; \
    then ln -sf python3 /usr/bin/python && \
	 python3 -m ensurepip && \
         pip3 install --no-cache --upgrade pip setuptools typed-argument-parser poetry pypdf; \
    fi
RUN if [ "$WITH_LATEX" = "true" ]; \
    then apk --no-cache --update add texmf-dist texlive-xetex texlive; \
    fi
RUN if [ "$WITH_DOC2PDF" = "true" ]; \
    then apk --no-cache --update add libreoffice python3 build-base gcc python3-dev ; \
    fi
USER node
