FROM redis:4.0.12

ARG HOST_USER_ID
ARG HOST_GROUP_ID

RUN if [ ${HOST_USER_ID:-0} -ne 0 ] && [ ${HOST_GROUP_ID:-0} -ne 0 ]; then \
userdel -f redis &&\
if getent group redis; then groupdel redis; fi &&\
groupadd -g ${HOST_GROUP_ID} redis &&\
useradd -l -u ${HOST_USER_ID} -g redis redis &&\
install -d -m 0755 -o redis -g redis /data &&\
chown --changes --silent --no-dereference --recursive \
  --from=999:999 ${HOST_USER_ID}:${HOST_GROUP_ID} \
  /data \
;fi
