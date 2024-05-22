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
  1. На базе образа NGINX последней версии создать катомный образ с измененным файлом index.html. В файле должен быть заготовок H1 с вашей ФИО  
  2. Запустить контейнер на порту 80  

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
