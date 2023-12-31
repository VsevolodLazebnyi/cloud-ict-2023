# Лабораторная работа 2

<div align="center">
  
```
    Состав команды:
  
    Боровский Максим (@unf1x)
    
    Лазебный Всеволод (@vllazebnyi)
```

<div align="left">

## Цель работы:

Написать два Dockerfile – плохой и хороший. Плохой должен запускаться и работать корректно, но в нём должно быть не менее 3 “bad practices”. В хорошем Dockerfile они должны быть исправлены. В Readme описать все плохие практики из кода Dockerfile и почему они плохие, как они были исправлены в хорошем  Dockerfile, а также две плохие практики по использованию этого контейнера

## Шаги по созданию хорошего Dockerfile:

1. Создадим директорию и инициализируем в ней проект с использованием Create React App (CRA) с шаблоном TypeScript:

   `npx create-react-app rctapp --template typescript`

2. Создаем Dockerfile с учетом хороших практик:

   2.1 Задаем базовый образ (используем node-alpine для уменьшения размера образа):

   ```
   FROM node:16.14.0-alpine
   ```

   2.2 Копируем содержимое в /app, исключая файлы, указанные в .dockerignore:

   ```
      WORKDIR /app

      COPY . /app
      COPY package.json package-lock.json /app/

   ```

    2.3 Устанавливаем зависимости из package-lock (чтобы избежать случайных изменений версий пакетов) и устанавливаем serve для     создания сервера:

   ```
      RUN npm install --package-lock-only && npm ci
      RUN npm install -g serve

      RUN npm run build

      CMD serve -s build
   ```

Добавляем файл .dockerignore, в который включаем избыточные файлы, которые не должны попасть в контейнер.

4. Собираем наш контейнер:

   `docker build -t lab-2 . `

5. Запускаем контейнер, привязывая порт приложения (3000) к желаемому порту на нашей машине (3001):

   `docker run  -p 3001:3000 lab-2 `

6. Проверяем, что приложение работает:

   ![12](https://github.com/VsevolodLazebnyi/cloud-ict-2023/blob/main/DEVOPS/lab-02/kartino4ki/l2.jpg?raw=true)

## Плохой Dockerfile

1. Использование устаревшего и необновляемого базового образа

   `FROM node:8.0`

2. Не установка рабочей директории:
   (В контейнер могут попасть ненужные файлы, которые не используются в приложении, что может увеличить размер образа.)
   
   `COPY . /`

4. Установка зависимостей без явного использования package-lock.json:
    `RUN npm install`

4.Использование CMD для запуска сервера без параметров
    `CMD serve`


