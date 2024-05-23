# DOCKER
Устанавливам машину на базе ОС ALT Linux.

Обновляем все пакеты:
```
apt-get update
```
Устанавливаем пакет Docker:
```
apt-get install -y docker-engine
```
Запускаем Docker:
```
systemctl enable --now docker
```
Скачиваем контейнер Hello-World:
```
docker pull hello-world
```
Смотрим список контейнеров:
```
docker images
```
Запускаем контейнер Hello-World:
```
docker run hello-world
```
Останавливаем контейнер:
```
docker rm -v *ID контейнера* 
```
Удаляем контейнер:
```
docker rmi *ID контейнера*
```

# Nginx в Docker  

## Задание 2  
__Цель задания:__  
  Запустить контейнер "NGINX" с мапингом порта 80. Сервер NGINX должен быть доступен с внешних устройств  

### Выполнение задания  
Обновляем пакеты:  
```
apt-get update
```
Далее загружаем `Nginx` контейнер в Docker:  
```
docker pull nginx
```
Запускаем контейнер на 80 порте:  
```
docker run --rm -d --anme nginx -v /data/app:/var/www/html -p 0.0.0.0:80:80 nginx
```
Проверяем запущенные контейнеры:  
```
docker ps
```

## Задание 3  
__Цель задания:__  
  1. На базе образа NGINX последней версии создать катомный образ с измененным файлом index.html. В файле должен быть заготовок H1 с вашей ФИО  
  2. Запустить контейнер на порту 80

### Выполнение задания  
Запускаем интерактивную оболочку в контейнере Docker с помощью docker exec с флагами -i и -t:
```
docker exec -it nginx /bin/bash
```
Поскольку это просто оболочка, здесь мы сможем только просматривать файлы в системе. 
Чтобы начать их редактировать нужно будет заново скачать программу, которой мы пользуемся для редака файлов, а именно `nano` или `vim`:
```
apt-get update
apt-get install -y nano
```
Далее необходимо открыть файл index.html:
```
nano /usr/share/nginx/html/index.html
```
В нём меняем заголовок и, по желанию, описание:

![image](https://github.com/Gogol15/Docker/assets/79337104/9521a416-fd40-4159-94c2-40d62c42f560)

## Задание 4  
__Цель задания:__  
  1. Создать образ для приложения работающего на фреймворке FastApi  
  2. Запустить контейнер на порту 80  

### Выполнение задания
Для начала обратимся к Python для создания нового окружения, которое мы назовём `docker`
```
pyton3 -m venv docker
```
Открываем его
```
source docker/bin/activate
```
В нём открываем файл `requirements.txt`, куда мы запишем наши зависимости
```
nano requirements.txt
```
Заполняем его следующим содержимым
```
fastapi>=0.68.0,<0.69.0
pydantic>=1.8.0,<2.0.0
uvicorn>=0.15.0,<0.16.0
```
И устанавливаем зависимости при помощи `pip`
```
pip install -r requirements.txt
```
Далее мы создаём новую директорию, называем её по своему усмотрению и заходим в неё
```
mkdir /fastapi
cd /fastapi
```
В ней создаём директорию `app`, переходим в неё и создаём 2 файла: `__init__.py` и `main.py`. Файл `__init__.py` оставляем пустым
```
mkdir /fastapi/app
cd /fastapi/app
nano /fastapi/app/__init__.py (сохраняем и выходим сочетаниями клавиш ctrl+s, ctrl+x)
nano /fastapi/app/main.py
```
Заполняем файл `main.py` следующим образом
```
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}
```
Далее в директории `/fastapi` создаём файл Dockerfile и заполняем его
```
>nano /fastapi/Dockerfile

FROM python:3.9
WORKDIR /code 
COPY ./requirements.txt /code/requirements.txt# 
RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt 
COPY ./app /code/app 
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "80"]
```
Не выходя из директории `/fastapi`, создаём в ней образ приложения FastAPI
```
docker build -t fastapi /fastapi
```
После прошедшей загрузки запускаем сам контейнер
```
docker run -d --name fastapi -p 80:80 fastapi
```
Чтобы проверить работу контейнера мы должны перейти по ссылке: http://IP_адрес_вашей_машины/items/5?q=somequery
Там мы увидим текст на подобии 
```
{"item_id": 5, "q": "somequery"}
```

## Задание 5
__Цель задания:__
  1. Запустить контейнер postgres, версия образа которого должна быть 16.3 
  2. Задать __имя контейнера__, __имя пользователя__ и __пароль пользователя__, указанного в задании
  3. Запустить контейнер на пору 5433

### Выполнение задания
Запускаем контейнер PostgreSQL со всеми нужными нам настройками
```
docker run --name college-pg -p 5433:5433 -e POSTGRES_USER=Student -e POSTGRES_PASSWORD=StudentPass postgres:16.3
```
Устанавливаем программу __pgAdmin__ и проверяем доступность нашей базы данных
