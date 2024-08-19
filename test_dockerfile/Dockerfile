
# Установим тег для базового образа чтобы процесс сборки всегда проходил в одинаковых условиях
# Если проект и все его зависимости совместимы с alpine то лучше использовать, так как это значительно уменьшит размер образа
FROM node:22.6.0

# Обединяем несколько RUN комманд в одну чтобы уменьшить колличество слоев Docker
# Отсортируем пакеты для установки в алфовитном порядке упращения обслуживания Dockerfile
# Очистим кэш для уменьшения размера образа
# Похорошему еще и прописать версии пакетов 
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

# Если переменную не нужно будет перезаписывать то можно отказаться от ARG и оставить только ENV (но это не кретично)
# ARG NODE_ENV=development
ENV NODE_ENV=development


# Также по хорошему иметь файл .dockerignore чтобы исключить копирование лишних файлов 
WORKDIR /opt/
COPY . .
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install


WORKDIR /opt/app
RUN chown -R node:node /opt/app
USER node


# Так как ENTRYPOINT и CMD выполняют одно и тоже можно избавиться от CMD
# их можно использовтаь вместе так как ENTRYPOINT - основная цель контейнера а CMD - поведение поумолчанию
# ENTRYPOINT - нельзя переопределить при запуске контейнера а CMD можно 
RUN ["yarn", "build"]
ENTRYPOINT ["yarn", "develop"]
# Однако ничего не запуститься так как нету package.json но если убрать yarn build и yarn develop то контейнер запускается 
# CMD ["yarn", "develop"]
