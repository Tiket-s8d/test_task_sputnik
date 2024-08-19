# Установим тег для базового образа, чтобы процесс сборки всегда проходил в одинаковых условиях
# Если проект и все его зависимости совместимы с Alpine, то лучше использовать его, так как это значительно уменьшит размер образа
FROM node:22.6.0

# Объединяем несколько RUN-команд в одну, чтобы уменьшить количество слоев Docker
# Отсортируем пакеты для установки в алфавитном порядке для упрощения обслуживания Dockerfile
# Очистим кэш для уменьшения размера образа
# Похоже, что также стоит прописать версии пакетов
RUN apt-get update && apt-get install --no-install-recommends -y \
autoconf \
automake \
bash \
build-essential \
gcc \
git \
libpng-dev \
libvips-dev \
nasm \
zlib1g-dev \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/*

# Если переменную не нужно будет перезаписывать, то можно отказаться от ARG и оставить только ENV (но это не критично)
# ARG NODE_ENV=development
ENV NODE_ENV=development

# Также по-хорошему иметь файл .dockerignore, чтобы исключить копирование лишних файлов
WORKDIR /opt/
COPY . .
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install

WORKDIR /opt/app
RUN chown -R node:node /opt/app
USER node

# Так как ENTRYPOINT и CMD выполняют одно и то же, можно избавиться от CMD
# Их можно использовать вместе, так как ENTRYPOINT — основная цель контейнера, а CMD — поведение по умолчанию
# ENTRYPOINT нельзя переопределить при запуске контейнера, а CMD можно
RUN ["yarn", "build"]
ENTRYPOINT ["yarn", "develop"]
# Однако ничего не запустится, так как нет package.json, но если убрать yarn build и yarn develop, то контейнер запускается
# CMD ["yarn", "develop"]
