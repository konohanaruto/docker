FROM mysql:5.7.24

ARG HOST_USER_ID
ARG HOST_GROUP_ID

RUN if [ ${HOST_USER_ID:-0} -ne 0 ] && [ ${HOST_GROUP_ID:-0} -ne 0 ]; then \
userdel -f mysql &&\
if getent group mysql; then groupdel mysql; fi &&\
groupadd -g ${HOST_GROUP_ID} mysql &&\
useradd -l -u ${HOST_USER_ID} -g mysql mysql &&\
install -d -m 0755 -o mysql -g mysql /var/lib/mysql &&\
chown --changes --silent --no-dereference --recursive \
  --from=999:999 ${HOST_USER_ID}:${HOST_GROUP_ID} \
  /var/lib/mysql \
;fi
