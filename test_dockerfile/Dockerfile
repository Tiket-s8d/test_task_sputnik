
# Установим тег для базового образа чтобы процесс сборки всегда проходил в одинаковых условиях
# Если проект и все его зависимости совместимы с alpine то лучше использовать, так как это значительно уменьшит размер образа
FROM node:22.6.0

# Обединяем несколько RUN комманд в одну чтобы уменьшить колличество слоев Docker
# Отсортируем пакеты для установки в алфовитном порядке упращения обслуживания Dockerfile
# Очистим кэш для уменьшения размера образа

RUN apt-get update && apt-get install -y \
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
&& rm -rf /var/lib/apt/lists/*

ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}


WORKDIR /opt/
COPY . .
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install


USER node
WORKDIR /opt/app

