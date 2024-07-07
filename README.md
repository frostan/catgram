# Проект Kittygram
 
 Дает возможность делиться картинками или фотографиями котиков, к сожалению для собак входа нет.
## Возможности:
Пользователь может зарегестрироваться на сервисе, а так же добавлять и удалять фотографии, но только своих публикаций.
Может делаиться достижениями своих любимцев.
## Kittygram_API:
Только зарегестрированным пользователям можно добавлять котиков.
 ## Технологии:
 ![Static Badge](https://img.shields.io/badge/Python-3.9-green)
![Static Badge](https://img.shields.io/badge/Django-green)
![Static Badge](https://img.shields.io/badge/REST_framework-red)
![Static Badge](https://img.shields.io/badge/Djoser-green)
![Static Badge](https://img.shields.io/badge/PosgreSQL-blue)
![Static Badge](https://img.shields.io/badge/Docker-lightblue)
![Static Badge](https://img.shields.io/badge/Nginx-gray)
![Static Badge](https://img.shields.io/badge/Gunicorn-white)
![Static Badge](https://img.shields.io/badge/Node.js-orange)
![Static Badge](https://img.shields.io/badge/JS-yellow)

## Запуск проекта на сервере на базе OC Linux.
Обновить пакетный менеджер
```
sudo apt update
sudo apt install curl
```
### Скачать Docker.
```
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin;
```
## Установка и настройка Nginx:
### Устанавливаем NGINX
```
sudo apt install nginx -y
```
### Запускаем
```
sudo systemctl start nginx
```
### Настраиваем firewall
```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
```
### Включаем firewall
```
sudo ufw enable
```
### Открываем конфигурационный файл NGINX
```
sudo nano /etc/nginx/sites-enabled/default
```
### Полностью удаляем из него все и пишем новые настройки
```
server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_set_header HOST $host;
        proxy_pass http://127.0.0.1:9000;

    }
}
```
### Проверяем корректность настроек
```
sudo nginx -t
```
### Запускаем NGINX
```
sudo systemctl start nginx
```

## Настройка протокола HTTPS:

### Установка пакетного менеджера snap.
```
sudo apt install snapd
```
### Установка и обновление зависимостей для пакетного менеджера snap.
```
sudo snap install core; sudo snap refresh core
```
 

### Установка пакета certbot.
```
sudo snap install --classic certbot
```
### Создание ссылки на certbot в системной директории,
### чтобы у пользователя с правами администратора был доступ к этому пакету.
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
### Получаем сертификат и настраиваем NGINX следуя инструкциям
```
sudo certbot --nginx
```
### Перезапускаем NGINX
```
sudo systemctl reload nginx
```

### Перейти в директорию проекта.
```
cd kittygram/
```
### Создать файл .env, и редактировать данные с пощью редактора nano.
```
sudo touch .env
sudo nano .env
```
### Редактируем данные и заменяем SECRET_KEY.
```
SECRET_KEY=<Смотреть ниже>
POSTGRES_DB=<Желаемое_имя_базы_данных>
POSTGRES_USER=<Желаемое_имя_пользователя_базы_данных>
POSTGRES_PASSWORD=<Желаемый_пароль_пользователя_базы_данных>
DB_HOST=db
DB_PORT=5432 стандартный порт БД
DEBUG=<Указать для продакшена - False, для разработки - True>
```
### Генерируем новый секретный ключ Django.
```
sudo docker compose -f docker-compose.production.yml exec backend python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
```
### Далее выполняем последовательно.
```
sudo docker compose -f docker-compose.production.yml pull
sudo docker compose -f docker-compose.production.yml down
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collect_static/. /backend_static/
```
### Создаем суперпользователся. Следуем инструкциям при выполнении.
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py createsuperuser
```

---

## Примеры запросов:
![Static Badge](https://img.shields.io/badge/GET-1fa7)
![Static Badge](https://img.shields.io/badge/POST-00BFFF)
![Static Badge](https://img.shields.io/badge/PATCH-FF8C00)
![Static Badge](https://img.shields.io/badge/DEL-FF0000)

### Получеие списка юзеров:

![Static Badge](https://img.shields.io/badge/GET-1fa7)```https://you.domain/api/users/```

#### Ответ: ```200```

```{
    "email": "string",
    "username": "string",
    "id": 0
}
```

## Автор проекта:
```
frostan
```