# https://raw.githubusercontent.com/vfauth/restic-backup-docker/restic-0.13/Dockerfile
FROM alpine:latest as rclone

# Get rclone executable
ADD https://downloads.rclone.org/rclone-current-linux-amd64.zip /
RUN unzip rclone-current-linux-amd64.zip && mv rclone-*-linux-amd64/rclone /bin/rclone && chmod +x /bin/rclone

FROM restic/restic:0.13.0

RUN apk add --update --no-cache heirloom-mailx fuse curl

COPY --from=rclone /bin/rclone /bin/rclone

RUN \
    mkdir -p /mnt/restic /var/spool/cron/crontabs /var/log; \
    touch /var/log/cron.log;

ARG RESTIC_REPOSITORY=/mnt/restic
ARG RESTIC_PASSWORD=""
ARG RESTIC_TAG=""
ARG NFS_TARGET=""
ARG BACKUP_CRON="0 */6 * * *"
ARG RESTIC_INIT_ARGS=""
ARG RESTIC_FORGET_ARGS=""
ARG RESTIC_JOB_ARGS=""
ARG MAILX_ARGS=""
ARG OS_AUTH_URL=""
ARG OS_PROJECT_ID=""
ARG OS_PROJECT_NAME=""
ARG OS_USER_DOMAIN_NAME="Default"
ARG OS_PROJECT_DOMAIN_ID="default"
ARG OS_USERNAME=""
ARG OS_PASSWORD=""
ARG OS_REGION_NAME=""
ARG OS_INTERFACE=""
ARG OS_IDENTITY_API_VERSION=3

ENV RESTIC_REPOSITORY=${RESTIC_REPOSITORY}
ENV RESTIC_PASSWORD=${RESTIC_PASSWORD}
ENV RESTIC_TAG=${RESTIC_TAG}
ENV NFS_TARGET=$NFS_TARGET
ENV BACKUP_CRON=${RESTIC_BACKUP_CRON}
ENV RESTIC_INIT_ARGS=${RESTIC_INIT_ARGS}
ENV RESTIC_FORGET_ARGS=$RESTIC_FORGET_ARGS
ENV RESTIC_JOB_ARGS=$RESTIC_JOB_ARGS
ENV MAILX_ARGS=$MAILX_ARGS
ENV OS_AUTH_URL=$OS_AUTH_URL
ENV OS_PROJECT_ID=$OS_PROJECT_ID
ENV OS_PROJECT_NAME=$OS_PROJECT_NAME
ENV OS_USER_DOMAIN_NAME=$OS_USER_DOMAIN_NAME
ENV OS_PROJECT_DOMAIN_ID=$OS_PROJECT_DOMAIN_ID
ENV OS_USERNAME=$OS_USERNAME
ENV OS_PASSWORD=$OS_PASSWORD
ENV OS_REGION_NAME=$OS_REGION_NAME
ENV OS_INTERFACE=$OS_INTERFACE
ENV OS_IDENTITY_API_VERSION=$OS_IDENTITY_API_VERSION

# openshift fix
RUN mkdir /.cache && \
    chgrp -R 0 /.cache && \
    chmod -R g=u /.cache && \
    chgrp -R 0 /mnt && \
    chmod -R g=u /mnt && \
    chgrp -R 0 /var/spool/cron/crontabs/root && \
    chmod -R g=u /var/spool/cron/crontabs/root && \
    chgrp -R 0 /var/log/cron.log && \
    chmod -R g=u /var/log/cron.log

# /data is the dir where you have to put the data to be backed up
VOLUME /data

COPY backup.sh /bin/backup
COPY entry.sh /entry.sh

WORKDIR "/"

RUN sed -i 's/$$RESTIC_INIT_ARGS/$RESTIC_INIT_ARGS/g'  /entry.sh
RUN sed -i 's/$$RESTIC_PASSWORD/$RESTIC_PASSWORD/g'  /entry.sh
ENTRYPOINT ["/entry.sh"]
CMD ["tail","-fn0","/var/log/cron.log"]
