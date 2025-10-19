<img width="1920" height="1080" alt="Снимок экрана (1663)" src="https://github.com/user-attachments/assets/f838e098-3e88-484a-882c-10ab7c9df87f" /># Домашнее задание к занятию 5. «Практическое применение Docker»

## Задача 1
Сделайте в своем GitHub пространстве fork репозитория.

Создайте файл Dockerfile.python на основе существующего Dockerfile:

Используйте базовый образ python:3.12-slim
Обязательно используйте конструкцию COPY . . в Dockerfile
Создайте .dockerignore файл для исключения ненужных файлов
Используйте CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "5000"] для запуска
Протестируйте корректность сборки
(Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker, с помощью venv. (Mysql БД можно запустить в docker run).

(Необязательная часть, *) Изучите код приложения и добавьте управление названием таблицы через ENV переменную.

## Ответ:

### Ссылка на fork: https://github.com/Dmitriy-py/shvirtd-example-python.git

<img width="1920" height="1080" alt="Снимок экрана (1638)" src="https://github.com/user-attachments/assets/93bfbf72-d166-4fdc-bfcf-6dda3b9008d1" />

<img width="1920" height="1080" alt="Снимок экрана (1642)" src="https://github.com/user-attachments/assets/063d8b3e-b4f3-43b2-a996-86fadfcbda05" />

## Задача 2 (*)

Создайте в yandex cloud container registry с именем "test" с помощью "yc tool" . Инструкция
Настройте аутентификацию вашего локального docker в yandex container registry.
Соберите и залейте в него образ с python приложением из задания №1.
Просканируйте образ на уязвимости.
В качестве ответа приложите отчет сканирования.

## Ответ:

<img width="1920" height="1080" alt="Снимок экрана (1651)" src="https://github.com/user-attachments/assets/2507c77c-e7d0-4b11-b90c-40a19b506c1a" />

<img width="1920" height="1080" alt="Снимок экрана (1653)" src="https://github.com/user-attachments/assets/344a49d0-fa41-4b01-91bb-bd232fb60b8f" />

<img width="1920" height="1080" alt="Снимок экрана (1654)" src="https://github.com/user-attachments/assets/251ac02a-0f89-4b0b-ac92-4df7c8b33cab" />


## Задача 3

Изучите файл "proxy.yaml"
Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
Опишите в файле compose.yaml следующие сервисы:
web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web

db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!

Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда curl -L http://127.0.0.1:8090 должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: docker ps -a  и docker logs <container_name> . Если вместо IP-адреса вы получаете информационную ошибку --убедитесь, что вы шлете запрос на порт 8090, а не 5000.

Подключитесь к БД mysql с помощью команды docker exec -ti <имя_контейнера> mysql -uroot -p<пароль root-пользователя>(обратите внимание что между ключем -u и логином root нет пробела. это важно!!! тоже самое с паролем) . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.

Остановите проект. В качестве ответа приложите скриншот sql-запроса.

## Ответ:

<img width="1920" height="1080" alt="Снимок экрана (1655)" src="https://github.com/user-attachments/assets/5a4ff449-0b2c-486b-bf2a-d78f7d72646f" />

```
“Первоначально целью было использование mysql:8, как указано в задании. Однако в моей среде разработки, а именно VirtualBox, образ mysql:8 демонстрировал нестабильное поведение при запуске. Проблема заключалась в долгом времени инициализации, из-за которого приложение не могло подключиться к базе данных вовремя, приводя к ошибкам 2003 Can't connect и 1146 Table doesn't exist.

Чтобы обеспечить соответствие требованиям задания и предоставить полностью работоспособное решение, я переключился на mariadb:10.6.4-focal. Этот образ оказался более надежным и быстрым в инициализации, что позволило приложению без проблем создать необходимые таблицы и установить соединение с базой данных. Таким образом, переход на MariaDB позволил успешно продемонстрировать все требования задания, избежав проблем с таймингами, характерных для mysql:8.”

```
## Задача 4

Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
Подключитесь к Вм по ssh и установите docker.
Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy. Трафик должен пройти через цепочки: Пользователь → Internet → Nginx → HAProxy → FastAPI(запись в БД) → HAProxy → Nginx → Internet → Пользователь

## Ответ:

### bash-скрипт ` deploy.sh `
```bash
#!/bin/bash
set -e

PROJECT_DIR="/opt/shvirtd-example-python"
REPO_URL="https://github.com/Dmitriy-py/shvirtd-example-python.git"
SERVICE_URL="http://127.0.0.1:8090"

echo "Starting deployment..."


if [ -d "$PROJECT_DIR" ]; then
    echo "Stopping existing project..."
    cd "$PROJECT_DIR"
    docker compose down
fi

echo "Pruning unused Docker resources..."
docker system prune -f
docker volume prune -f
docker network prune -f


if [ -d "$PROJECT_DIR" ]; then
    sudo rm -rf "$PROJECT_DIR"
fi

echo "Cloning repository from $REPO_URL to /opt..."
sudo git clone "$REPO_URL" /opt/shvirtd-example-python

sudo chown -R $USER:$USER "$PROJECT_DIR"

echo "Building and starting containers..."
cd "$PROJECT_DIR"

if [ ! -f "$PROJECT_DIR/.env" ]; then
    echo "ERROR: .env file not found in $PROJECT_DIR. Please create it manually."
    exit 1
fi

docker compose up -d --build

echo "Waiting 60 seconds for service initialization..."
sleep 60

STATUS_CODE=$(curl -s -o /dev/null -w "%{http_code}" $SERVICE_URL)

if [ "$STATUS_CODE" -eq 200 ]; then
    echo "SUCCESS: Service is reachable at $SERVICE_URL (HTTP $STATUS_CODE)"
else
    echo "FAILURE: Service check failed (HTTP $STATUS_CODE)."
    echo "Checking Docker logs..."
    docker logs mysql-db-compose || true
    docker logs web-app-compose || true
    exit 1
fi

echo "Deployment finished successfully."

```

<img width="1920" height="1080" alt="Снимок экрана (1656)" src="https://github.com/user-attachments/assets/71127005-8b77-4edd-8314-662f94214faa" />

<img width="1920" height="1080" alt="Снимок экрана (1657)" src="https://github.com/user-attachments/assets/9c2e0737-decb-4969-85d2-3360a73f494a" />

<img width="1920" height="1080" alt="Снимок экрана (1658)" src="https://github.com/user-attachments/assets/c06bd3ca-8e8c-4973-a90d-fc21b43c0b11" />

<img width="1920" height="1080" alt="Снимок экрана (1659)" src="https://github.com/user-attachments/assets/60ebbc4f-0396-4a22-8d5a-a0bad7d2bd6e" />


(Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a
Повторите SQL-запрос на сервере и приложите скриншот и ссылку на fork.

## Ответ:

<img width="1920" height="1080" alt="Снимок экрана (1660)" src="https://github.com/user-attachments/assets/c86dc5ae-59cb-4cd5-a0da-569c06b31ca2" />

### Ссылка на fork: https://github.com/Dmitriy-py/shvirtd-example-python.git


## Задача 6

Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save. Предоставьте скриншоты действий .

## Ответ:

<img width="1920" height="1080" alt="Снимок экрана (1663)" src="https://github.com/user-attachments/assets/e85d6b58-93e6-48bc-98cf-9e6c7dfd5e51" />












