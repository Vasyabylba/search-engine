<h1 align="center">Search Engine</h1>

<p align="center">Поисковый движок представляет из себя Spring-Boot приложение, работающее с локально установленной базой данных MySQL, имеющее простой веб-интерфейс и API, через который им можно управлять и получать результаты поисковой выдачи по запросу.</p>

<hr>

## Overview

Веб-интерфейс приложения представляет собой одну веб-страницу с тремя вкладками.

### Dashboards

Эта вкладка открывается по умолчанию. На ней отображается общая статистика по всем сайтам, а также детальная статистика
и статус по каждому из сайтов.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/dashboards_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/dashboards_light.gif">
  <img alt="Shows dashboards-light.gif in light mode and dashboards-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/dashboards_light.gif">
</picture>

### Management

На этой вкладке находятся инструменты управления поисковым движком — запуск и остановка полной индексации (переиндексации),
а также возможность добавить (обновить) отдельную страницу по ссылке.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/management_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/management_light.gif">
  <img alt="Shows management-light.gif in light mode and management-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/management_light.gif">
</picture>

### Search

Данная вкладка предназначена для поиска страниц. На ней находятся выпадающий список с выбором
сайта для поиска и поле поиска, а при нажатии на кнопку «Search» происводится поиск 
с последующей выдачей результатов.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/search_dark.gif">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/search_light.gif">
  <img alt="Shows search-light.gif in light mode and search-dark.gif in dark mode." src="https://raw.githubusercontent.com/Vasyabylba/searchengine/dev/readme_assets/search_light.gif">
</picture>

## API

* GET /api/statistics — статистика
* GET /api/startIndexing — запуск полной индексации
* GET /api/stopIndexing — остановка текущей индексации
* POST /api/indexPage — добавление или обновление отдельной страницы
* GET /api/search — получение данных по поисковому запросу

### GET /api/statistics

Метод возвращает статистику и другую служебную информацию о состоянии поисковых индексов и самого движка.

### GET /api/startIndexing

Метод запускает полную индексацию всех сайтов или полную переиндексацию, если они уже проиндексированы.

### GET /api/stopIndexing

Метод останавливает текущий процесс индексации (переиндексации).

### POST /api/indexPage

Метод добавляет в индекс или обновляет отдельную страницу, адрес которой передан в параметре.

Параметры:
* url — адрес страницы, которую необходимо добавить в индекс или переиндексировать

### GET /api/search

Метод осуществляет поиск страниц по переданному поисковому запросу (параметр query).
Чтобы получить результаты порционно, можно задать параметры offset (сдвиг от начала списка результатов)
и limit (количество результатов, которое необходимо вывести).

Параметры:
* query — поисковый запрос
* site — сайт, по которому осуществлять поиск (если не задан, поиск происходит по всем проиндексированным сайтам);
задаётся в формате адреса, например: http://www.site.com (без слэша в конце)
* offset — сдвиг от начала списка результатов для постраничного вывода (параметр необязательный;
если не установлен, то значение по умолчанию равно 0)
* limit — количество результатов, которое необходимо вывести (параметр необязательный;
если не установлен, то значение по умолчанию равно 20)

## How it Works?

1. В конфигурационном файле `./application.yml` перед запуском приложения задаются
   адреса и названия сайтов, по которым приложение должно осуществлять поиск.
2. Приложение в многопоточном режиме обходит все страницы заданных сайтов 
   и индексирует их (создает так называемый индекс) так, чтобы потом находить 
   наиболее релевантные страницы по поисковому запросу.
3. Пользователь присылает поисковой запрос через API приложения.
4. Запрос определённым образом трансформируется в список слов,
   переведённых в базовую форму. Например, для существительных —
   именительный падеж, единственное число.
5. В индексе ищутся страницы, на которых встречаются эти слова.
6. Результаты поиска ранжируются, сортируются и отдаются пользователю.

## Technical stack

* Java 17
* Spring Boot 2.7.1
* Spring Data JPA 2.7.1
* MySQL 8.0
* Maven
* HTML5
* JavaScript
* CSS3
* Docker

## Install / How to use it?

### Настройки подключения к БД

В приложение добавлен драйвер для подключения к БД MySQL. Для запуска приложения,
необходимо убедиться, что у вас запущен сервер MySQL 8.x.

🐳 Если у вас установлен докер, можете запустить контейнер с готовыми настройками
под приложение командой:

```bash
docker run -d --name search_engine -e "MYSQL_ROOT_PASSWORD=Mysqlpass" -e "MYSQL_DATABASE=search_engine" -p 3306:3306 mysql --character-set-server=utf8mb4 --collation-server=utf8mb4_general_ci
```

Имя пользователя по-умолчанию `root`, настройки проекта в `./application.yml`
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
java -jar search_engine.jar
```