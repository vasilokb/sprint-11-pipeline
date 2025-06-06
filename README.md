# Sprint 11 Pipeline

## Описание проекта

Проект `sprint-11-pipeline` представляет собой конвейер обработки данных, построенный с использованием современных инструментов для управления данными, таких как Apache Kafka, DataHub, PostgreSQL, MinIO, Nessie и Dremio. Основная цель проекта — обеспечить сбор, хранение и обработку данных о пользователях (например, подписках Netflix) в масштабируемой и управляемой среде.

Проект включает:
- **Базу данных PostgreSQL** для хранения данных о пользователях.
- **Apache Kafka** для потоковой обработки данных.
- **DataHub** для управления метаданными.
- **MinIO** как объектное хранилище.
- **Nessie** как каталог данных.
- **Dremio** для аналитики и управления данными.

Все сервисы настроены через `docker-compose.yml` для упрощения развертывания.

## Структура проекта

- **`db/`** — папка с файлами для инициализации базы данных:
  - `users.csv` — CSV-файл с данными о пользователях, содержащий следующие поля:
    - `User ID`
    - `Subscription Type`
    - `Monthly Revenue`
    - `Join Date`
    - `Last Payment Date`
    - `Country`
    - `Age`
    - `Gender`
    - `Device`
    - `Plan Duration`
  - `init-db.sql` — SQL-скрипт для создания таблицы `users` в PostgreSQL и импорта данных из `users.csv`.
- **`docker-compose.yml`** — файл конфигурации Docker Compose, который описывает все сервисы проекта, включая:
  - PostgreSQL
  - Apache Kafka (с Zookeeper и Schema Registry)
  - DataHub (GMS, Frontend, Actions)
  - Elasticsearch
  - MinIO (объектное хранилище)
  - Nessie (каталог данных)
  - Dremio (аналитика данных)

## Требования

Для запуска проекта необходимы:
- **Docker** (версия 20.10 или выше)
- **Docker Compose** (версия 2.0 или выше)
- Операционная система: Linux, macOS или Windows с WSL2
- Минимум 8 ГБ оперативной памяти для корректной работы всех сервисов

## Установка и запуск

1. **Клонируйте репозиторий**:
   ```bash
   git clone https://github.com/vasilokb/sprint-11-pipeline.git
   cd sprint-11-pipeline
   ```

2. **Запустите сервисы с помощью Docker Compose**:
   ```bash
   docker-compose up -d
   ```
   Флаг `-d` запускает сервисы в фоновом режиме.

3. **Проверка работоспособности**:
   - **PostgreSQL**: Подключитесь к базе данных (`netflix`) с пользователем `airflow` и паролем `airflow`:
     ```bash
     docker exec -it <postgres_container_name> psql -U airflow -d netflix
     ```
     Проверьте, что таблица `users` создана и содержит данные.
   - **DataHub**: Откройте интерфейс DataHub в браузере по адресу `http://localhost:9002` (логин: `datahub`, пароль: `datahub`).
   - **MinIO**: Доступ к интерфейсу MinIO по адресу `http://localhost:9001` (логин: `admin`, пароль: `password`).
   - **Dremio**: Интерфейс Dremio доступен по адресу `http://localhost:9047`.
   - **Nessie**: API каталога доступно по адресу `http://localhost:19120`.

4. **Остановка сервисов**:
   ```bash
   docker-compose down
   ```

## Использование

- **PostgreSQL**: Хранит данные о пользователях, которые можно анализировать через SQL-запросы.
- **Kafka**: Используется для передачи событий между сервисами (например, метаданные или изменения данных).
- **DataHub**: Управляет метаданными, позволяя отслеживать происхождение и структуру данных.
- **MinIO**: Хранит файлы и результаты обработки в объектном хранилище.
- **Nessie**: Обеспечивает версионирование данных и управление каталогом.
- **Dremio**: Позволяет выполнять аналитические запросы к данным из различных источников.

## Дополнительные замечания

- Убедитесь, что порты, указанные в `docker-compose.yml` (например, 9092, 9002, 9000, 9047), не заняты другими приложениями.
- Для настройки переменных окружения (например, `DATAHUB_VERSION`) создайте файл `.env` в корне проекта.
- Для масштабирования или настройки дополнительных параметров Kafka или DataHub обратитесь к официальной документации этих инструментов.
