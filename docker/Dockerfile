FROM node:lts-alpine3.12  
ARG QL_MAINTAINER="whyour"  
LABEL maintainer="${QL_MAINTAINER}"  
ARG QL_URL=https://github.com/${QL_MAINTAINER}/qinglong.git  
ARG QL_BRANCH=master  
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \  
    LANG=zh_CN.UTF-8 \  
    SHELL=/bin/bash \  
    PS1="\u@\h:\w \$ " \  
    QL_DIR=/ql \  
    QL_BRANCH=${QL_BRANCH}  
WORKDIR ${QL_DIR}  
RUN set -x \  
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \  
    && echo "Completed: Changed APK repositories to Aliyun" \  
    && apk update -f \  
    && echo "Completed: APK update" \  
    && apk upgrade \  
    && echo "Completed: APK upgrade" \  
    && apk --no-cache add -f bash \  
                             coreutils \  
                             moreutils \  
                             git \  
                             curl \  
                             wget \  
                             tzdata \  
                             perl \  
                             openssl \  
                             nginx \  
                             python3 \  
                             jq \  
                             openssh \  
                             py3-pip \  
                             python2 \  
                             g++ \  
                             make \  
                             sqlite \  
                             sqlite-dev \  
    && echo "Completed: Installed necessary packages" \  
    && wget https://github.com/TryGhost/node-sqlite3/archive/v5.0.2.zip -O /ql/sqlite3.zip \  
    && echo "Completed: Downloaded node-sqlite3" \  
    && mkdir -p /ql/sqlite3 \  
    && mkdir -p /ql/node_modules/sqlite3 \  
    && unzip /ql/sqlite3.zip -d /ql/sqlite3 \  
    && echo "Completed: Unzipped node-sqlite3" \  
    && cd /ql/sqlite3/node-sqlite3-5.0.2 \  
    && npm install \  
    && echo "Completed: Installed node-sqlite3 dependencies" \  
    && ./node_modules/.bin/node-pre-gyp install --fallback-to-build --build-from-source --sqlite=/usr/bin --python=$(which python) \  
    && echo "Completed: Built node-sqlite3 from source" \  
    && mv /ql/sqlite3/node-sqlite3-5.0.2 /ql/node_modules/sqlite3 \  
    && apk del g++ make python2 \  
    && echo "Completed: Removed unnecessary build dependencies" \  
    && rm -Rf /ql/sqlite3 /ql/sqlite3.zip \  
    && echo "Completed: Cleaned up sqlite3 build files" \  
    && rm -rf /var/cache/apk/* \  
    && echo "Completed: Cleaned up APK cache" \  
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \  
    && echo "Completed: Set timezone to Asia/Shanghai" \  
    && echo "Asia/Shanghai" > /etc/timezone \  
    && touch ~/.bashrc \  
    && git clone -b ${QL_BRANCH} ${QL_URL} ${QL_DIR} \  
    && echo "Completed: Cloned Qinglong repository" \  
    && git config --global user.email "qinglong@@users.noreply.github.com" \  
    && git config --global user.name "qinglong" \  
    && cd ${QL_DIR} \  
    && cp -f .env.example .env \  
    && echo "Completed: Copied .env.example to .env" \  
    && chmod 777 ${QL_DIR}/shell/*.sh \  
    && chmod 777 ${QL_DIR}/docker/*.sh \  
    && echo "Completed: Made scripts executable" \  
    && npm install -g pnpm \  
    && echo "Completed: Installed pnpm globally" \
    && pnpm install -g pm2 \
    && pnpm install -g ts-node typescript tslib \
    && rm -rf /root/.npm \
    && pnpm install --prod \
    && rm -rf /root/.pnpm-store \
    && git clone -b ${QL_BRANCH} https://github.com/${QL_MAINTAINER}/qinglong-static.git /static \
    && cp -rf /static/* ${QL_DIR} \
    && rm -rf /static
ENTRYPOINT ["./docker/docker-entrypoint.sh"]
