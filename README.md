![изображение](https://github.com/user-attachments/assets/b0414e15-304d-4e1f-a7bf-9c8944dd638f)# Описание разделов README
- Описание по запуску:
  - Способ 1. docker-compose:
    - 1.1. На Linux
    - 1.2. На Windows
  - Способ 2. intellij idea
  - Способ 3. Через запуск JAR файлов с заданием переменных окружения
    - 3.1 На Linux
    - 3.2 На Windows

- Доступ ко Swagger
- Описание, где посмотреть исходники
- Дополнительные функциональные требования

# Описание по запуску

## Способ 1. docker-compose

### На Linux
1. **Необходимо** скачать архив с `volume` с JAR файлами [по этой ссылке](https://drive.google.com/file/d/1BPfEaaM9GeklKWGKh3cTCtoYJ4CHPFnE/view?usp=sharing), нет возможности загружать файлы > 100 МБ на `Github`. Распаковать архив любым способом.
2. Перейти в терминале в ту директорию, где распологается корень распокованного архива.
3. Убедиться, что порты 8081, 8082 и 8089 свободны. Например, выполнив команду ниже и убедиться, что она не выдала результата.
```bash
sudo ss -tuln | grep -E '8081|8082|8089'
```
4. Выполнить следующие команды в терминале от имени привилегированного пользователя (добавив вначале каждый команд `sudo` или открыв терминал от имени `root`):
```bash
docker load -i jar-files.tar
docker-compose up --build
```
5. Ожидать, пока в терминале не остановятся появляться новые секунды на протяжении нескольких секунд, и пока не появятся 4 записи `Tomcat started on port`: 
![изображение](https://github.com/user-attachments/assets/2d05078e-4fec-4e72-95b4-4a0e0ef5e5b8)
6. Для доступа ко фронтенду перейти на `localhost:8081`. Для доступа к REST API к `localhost:8082`. Для доступа к БД Postgres использовать `localhost:8089`, login=password=`postgres`, название БД - `testtask-2025-03-05`, порт стандартный - `5432`.

### На Windows

Будет

## Способ 2. Intellij IDEA
1. Клинировать рекурсивно [этот репозиторий](https://github.com/WebCoder1980/test_task_2025-03-05-main)
```
git clone https://github.com/WebCoder1980/test_task_2025-03-05-main --recurse-submodules
```

2. Открыть каждый из проектов в отдельном проекте Intellij IDEA, который имеет суффикс `-microservice` (их 4).
3. Задать в Intellij IDEA переменные окружения для каждого проекта, нажав на изменение конфигураций:
![изображение](https://github.com/user-attachments/assets/0b4789b5-baed-4851-ac06-8a17daeb240f)
![изображение](https://github.com/user-attachments/assets/4f06bb4b-670b-4023-88a0-3c74bf8c8e76)

**Требуется** поменять `POSTGRES_DB`, `POSTGRES_HOST`, `POSTGRES_LOGIN`, `POSTGRES_PASSWORD`, `POSTGRES_PORT` на те значения, на которых есть рабочая БД Postgres:
```
POSTGRES_DB=testtask-2025-03-05;POSTGRES_HOST=postgres;POSTGRES_LOGIN=postgres;POSTGRES_PASSWORD=postgres;POSTGRES_PORT=5432;EMPLOYEE_MICROSERVICE_HOST=localhost;EMPLOYEE_MICROSERVICE_PORT=8083;SHOP_MICROSERVICE_HOST=localhost;SHOP_MICROSERVICE_PORT=8084;ELECTROITEM_MICROSERVICE_HOST=localhost;ELECTROITEM_MICROSERVICE_PORT=8085;PURCHASE_MICROSERVICE_HOST=localhost;PURCHASE_MICROSERVICE_PORT=8086;SPRING_PORT=8082;POSTGRES_USER=postgres
```
4. Запустить каждый проект.
5. Дождаться, пока не выведится `Tomcat started on port` в каждом из проектов.
6. Смотреть раздел **Как пользоваться приложением**

## Способ 3.1. Java CLI с заданием переменных окружения c inline environment variable assignment:

### 3.1 На Linux
1. Клонируйте репозиторий с JAR и frontend [по этой ссылке](https://github.com/WebCoder1980/test_task_2025-03-05-run)
2. Любым способом задайте переменные окружения. Например:
```bash
export POSTGRES_DB="testtask-2025-03-05" # Укажите данные от рабочей БД Postgres
export POSTGRES_HOST="postgres" # Укажите данные от рабочей БД Postgres
export POSTGRES_PORT="5432" # Укажите данные от рабочей БД Postgres
export POSTGRES_LOGIN="postgres" # Укажите данные от рабочей БД Postgres
export POSTGRES_PASSWORD="postgres" # Укажите данные от рабочей БД Postgres

export EMPLOYEE_MICROSERVICE_HOST="localhost"
export EMPLOYEE_MICROSERVICE_PORT="8083"

export SHOP_MICROSERVICE_HOST="localhost"
export SHOP_MICROSERVICE_PORT="8084"

export ELECTROITEM_MICROSERVICE_HOST="localhost"
export ELECTROITEM_MICROSERVICE_PORT="8085"

export PURCHASE_MICROSERVICE_HOST="localhost"
export PURCHASE_MICROSERVICE_PORT="8086"

export SPRING_PORT="8082"
```

3. Перед тем как пытаться запустить приложения убедитесь, что порты `8082`, `8083`, `8084`, `8085`, `8086` свободны:
```
sudo ss -tuln | grep -E '8082|8083|8084|8085|8086'
```

Что бы убить процессы на этих портах (по крайней мере когда потребуется возможность прекратить выполнения приложения следующими командами):
```
fuser -k 8082/tcp 8083/tcp 8084/tcp 8085/tcp 8086/tcp
```

Запустите экземпляры микросервисов приложения:
```
java -jar gateway-microservice-1.0.0.jar & java -jar employee-microservice-1.0.0.jar & java -jar shop-microservice-1.0.0.jar & java -jar electroitem-microservice-1.0.0.jar & java -jar purchase-microservice-1.0.0.jar
```
Или открыв каждое приложение в отдельном терминале без `&`, введя по одной строчки команды в каждый терминал:
```
java -jar gateway-microservice-1.0.0.jar
java -jar employee-microservice-1.0.0.jar
java -jar shop-microservice-1.0.0.jar
java -jar electroitem-microservice-1.0.0.jar
java -jar purchase-microservice-1.0.0.jar
```

4. Ожидать, пока в терминале (или терминалах) не остановятся появляться новые секунды на протяжении нескольких секунд, и пока не появятся 4 записи `Tomcat started on port`: 
![изображение](https://github.com/user-attachments/assets/2d05078e-4fec-4e72-95b4-4a0e0ef5e5b8)

5. Смотреть раздел **Как пользоваться приложением**

## 3.2 На Windows
1. Добавить переменные окружения любым способом, например [по этой ссылке](https://remontka.pro/environment-variables-windows/)
2. Клонируйте репозиторий с JAR и frontend [по этой ссылке](https://github.com/WebCoder1980/test_task_2025-03-05-run)
3. Запустите каждый jar файл в отдельном терминале:
```
java -jar <название файла>
```

4. Ожидать, пока в терминале (или терминалах) не остановятся появляться новые секунды на протяжении нескольких секунд, и пока не появятся 4 записи `Tomcat started on port`: 
![изображение](https://github.com/user-attachments/assets/2d05078e-4fec-4e72-95b4-4a0e0ef5e5b8)

5. Открыть `index.html` в директории `frontend/`

# Описание, где посмотреть исходники

В этом репозитории, где и данный `README`:
```
git clone https://github.com/WebCoder1980/test_task_2025-03-05-main --recurse-submodules
```

# Дополнительные функциональные требования

```
    1. Реализовать вывод информации о лучших сотрудниках в зависимости от занимаемой должности по следующим критериям:
    • Количество проданных товаров за последний год.
    • Сумма проданных товаров. Прописать обозначения за последний год.
    2. Реализовать вывод информации по определенным критериям:
    • вывод лучшего младшего продавца-консультанта, продавшего больше всех умных часов;
    • вывод суммы денежных средств, полученной магазином через оплату наличными.
```
Сделано на главной странице - `index.html`
