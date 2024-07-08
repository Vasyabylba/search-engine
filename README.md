<h1 align="center">Search Engine</h1>

<p align="center">
Поисковый движок представляет из себя Spring Boot приложение, работающее с локально установленной базой данных MySQL. 
Приложение имеет веб-интерфейс и API, через который им можно управлять и осуществлять поиск информации по 
запросу.</p>

<hr>

* [Overview](#overview)
    * [Web interface](#web-interface)
        * [Dashboards](#dashboards)
        * [Management](#management)
        * [Search](#search)
* [Technical stack](#technical-stack)
* [Documentation](#documentation)
    * [API specification](#api-specification)
        * [GET /api/statistics](#get-apistatistics)
        * [GET /api/startIndexing](#get-apistartindexing)
        * [GET /api/stopIndexing](#get-apistopindexing)
        * [POST /api/indexPage](#post-apiindexpage)
        * [GET /api/search](#get-apisearch)
* [How it Works?](#how-it-works)
* [Install / How to use it?](#install--how-to-use-it)
    * [Настройки подключения к БД](#настройки-подключения-к-бд)
    * [Настройки приложения](#настройки-приложения)
    * [Запуск приложения](#запуск-приложения)

## Overview

### Web interface

Веб-интерфейс приложения представляет собой одну веб-страницу с тремя вкладками.

#### Dashboards

Эта вкладка открывается по умолчанию. На ней отображается общая статистика по всем сайтам, а также детальная статистика
и статус по каждому из сайтов.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/dashboards_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/dashboards_light.gif">
  <img alt="Shows dashboards-light.gif in light mode and dashboards-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/dashboards_light.gif">
</picture>

#### Management

На этой вкладке находятся инструменты управления поисковым движком — запуск и остановка полной индексации (
переиндексации), а также возможность добавить (обновить) отдельную страницу по ссылке.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/management_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/management_light.gif">
  <img alt="Shows management-light.gif in light mode and management-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/management_light.gif">
</picture>

#### Search

Данная вкладка предназначена для поиска страниц. На ней находятся выпадающий список с выбором сайта для поиска и
поле поиска, а при нажатии на кнопку «Search» производится поиск с последующей выдачей результатов.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/search_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/search_light.gif">
  <img alt="Shows search-light.gif in light mode and search-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/master/readme_assets/search_light.gif">
</picture>

## Technical stack

<ul>
  <li>Java 17</li>
  <li>Spring Boot</li>
  <li>Spring Web MVC</li>
  <li>Spring Data JPA</li>
  <li>Hibernate</li>
  <li>JSOUP</li>
  <li>Apache Lucene Morphology</li>
  <li>MySQL</li>
  <li>Maven</li>
  <li>HTML5</li>
  <li>JavaScript</li>
  <li>CSS3</li>
  <li>Docker</li>
</ul>

## Documentation

Swagger (OpenAPI 3.0): http://localhost:8080/swagger-ui/index.html

### API specification

| Method | URI                  | Description                                          |
|--------|----------------------|------------------------------------------------------|
| `GET`  | `/api/statistics`    | Возвращает статистику и другую служебную информацию. |
| `GET`  | `/api/startIndexing` | Запускает полную индексацию.                         |
| `GET`  | `/api/stopIndexing`  | Останавливает текущий процесс индексации.            |
| `POST` | `/api/indexPage`     | Добавляет в индекс или обновляет отдельную страницу. |
| `GET`  | `/api/search`        | Получает данные по поисковому запросу.               |

#### `GET` /api/statistics

Возвращает статистику и другую служебную информацию о состоянии поисковых индексов и поисковой системы.

#### `GET` /api/startIndexing

Запускает полную индексацию всех сайтов или полную переиндексацию, если они уже проиндексированы.

#### `GET` /api/stopIndexing

Останавливает текущий процесс индексации или переиндексации.

#### `POST` /api/indexPage

Добавляет в индекс или обновляет отдельную страницу, адрес которой передан в параметре.

**Параметры:**

* `url` — адрес страницы, которую необходимо добавить в индекс или переиндексировать

#### `GET` /api/search

Осуществляет поиск страниц по переданному поисковому запросу (параметр query).
Чтобы получить результаты порционно, можно задать параметры `offset` (сдвиг от начала списка результатов)
и `limit` (количество результатов, которое необходимо вывести).

**Параметры:**

* `query` — поисковый запрос
* `site` — сайт, по которому осуществлять поиск. Задаётся в формате адреса, например: http://www.site.com (без слэша
  в конце). Если не задан, поиск происходит по всем проиндексированным сайтам.
* `offset` — сдвиг от начала списка результатов для постраничного вывода (параметр необязательный;
  если не установлен, то значение по умолчанию равно 0)
* `limit` — количество результатов, которое необходимо вывести (параметр необязательный;
  если не установлен, то значение по умолчанию равно 20)

## How it Works?

1. В конфигурационном файле `./application.yml` перед запуском приложения задаются
   адреса и названия сайтов, по которым приложение должно осуществлять поиск.
2. Пользователь запускает индексацию через API приложения.
3. Приложение в многопоточном режиме обходит все страницы заданных сайтов
   и индексирует их (создает так называемый индекс) так, чтобы потом находить
   наиболее релевантные страницы по поисковому запросу.
4. Пользователь присылает поисковой запрос через API приложения.
5. Запрос определённым образом трансформируется в список слов,
   переведённых в базовую форму. Например, для существительных — это
   именительный падеж, единственное число.
6. В индексе ищутся страницы, на которых встречаются данные слова.
7. Результаты поиска ранжируются, сортируются и отдаются пользователю.

## Install / How to use it?

**Склонируйте репозиторий:**

```bash
git clone https://github.com/vasyabylba/search-engine.git
cd search-engine
```

### Настройки подключения к БД

В приложение добавлен драйвер для подключения к БД MySQL. Для запуска приложения,
необходимо убедиться, что у вас запущен сервер MySQL 8.x.

🐳 Если у вас установлен докер, можете запустить контейнер с готовыми настройками
под приложение командой:

```bash
docker run -d --name search_engine -e "MYSQL_ROOT_PASSWORD=Mysqlpass" -e "MYSQL_DATABASE=search_engine" -p 3306:3306 mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
```

Имя пользователя по умолчанию `root`, настройки проекта в `./application.yml`
соответствуют настройкам контейнера, менять их не требуется.

❗️ Если у вас MacBook c процессором M series, необходимо использовать специальный
образ для ARM процессоров:

```bash
docker run -d --name search_engine -e "MYSQL_ROOT_PASSWORD=Mysqlpass" -e "MYSQL_DATABASE=search_engine" -p 3306:3306 arm64v8/mysql:oracle --character-set-server=utf8mb4 --collation-utf8mb4_general_ci
```

При использовании MySQL без докера, необходимо создайть БД `search_engine` и заменить логин и пароль
в файле конфигурации `./application.yml`:

```yaml
spring:
  datasource:
    username: root # имя пользователя
    password: Mysqlpass # пароль пользователя
```

### Настройки приложения

Список сайтов для которых будет осуществляться индексация необходимо указать в файле конфигурации
`./application.yml`, путём указания адреса (без слэша в конце) и названия для каждого сайта:

```yaml
indexing-settings:
  sites:
    - url: https://www.site.com # адрес сайта
      name: SiteName # название сайта
```

### Запуск приложения

```bash
java -jar search-engine.jar
```