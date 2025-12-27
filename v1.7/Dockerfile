# Используем официальный Amazon Corretto 8 (OpenJDK 8)
FROM public.ecr.aws/amazoncorretto/amazoncorretto:8

# Версия Allure, которую хотим установить
ARG RELEASE=2.32.0
ARG ALLURE_REPO=https://repo.maven.apache.org/maven2/io/qameta/allure/allure-commandline

# Обновляем систему и устанавливаем нужные утилиты
RUN yum -y update && \
    yum -y install tar wget gzip && \
    rm -rf /var/cache/yum/*

# Скачиваем Allure и распаковываем
RUN wget --no-verbose -O /tmp/allure-$RELEASE.tgz $ALLURE_REPO/$RELEASE/allure-commandline-$RELEASE.tgz && \
    tar -zxvf /tmp/allure-$RELEASE.tgz -C / && \
    rm -rf /tmp/* && \
    chmod -R +x /allure-$RELEASE/bin

# Настройка переменных окружения
ENV ROOT=/app \
    PATH=$PATH:/allure-$RELEASE/bin

# Создаём рабочую директорию для Action
RUN mkdir -p $ROOT
WORKDIR $ROOT

# Копируем entrypoint скрипт
COPY ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Точка входа
ENTRYPOINT ["/entrypoint.sh"]
