# RServer RedOS
## Оглавление

1. [Архитектура](#архитектура)
    - [Общий порядок взаимодействия](#общий-порядок-взаимодействия)
    - [Порядок работы пользователя с документом](#порядок-работы-пользователя-с-документом)
2. [Преимущества](#преимущества)
3. [Технологии](#технологии)
4. [Установка](#установка)
   - [Подтверждение установки](#подтверждение-установки)
6. [Контакты](#контакты)

## Архитектура

### Общий порядок взаимодействия
- **Web**: Веб-сервер для обработки клиентских запросов.
- **Document Server**: Отдельный сервер для редактирования документов.
- **API**: Программный интерфейс для взаимодействия между компонентами.
- **PostgreSQL**: БД для хранения данных документов и пользовательских данных.
- **RabbitMQ**: Брокер сообщений для асинхронного взаимодействия.
- **FileStorage**: REST API для доступа к файловому хранилищу.
- **Service Registry**: Реестр сервисов для управления микросервисами.
- **Search Server BsaSearch**: Поисковый сервер для индексации и поиска.
- **Processing**: Компонент для индексации данных.

### Порядок работы пользователя с документом
1. Пользователь открывает документ в веб-интерфейсе.
2. После проверки прав доступа, пользователю передается ссылка на файл.
3. Доступны два варианта сохранения: при закрытии документа и периодическое сохранение.

## Преимущества

- Горизонтальная масштабируемость
- Отказоустойчивость
- Использование безопасных и современных ОС
- Гибкость технологий
- Stateless архитектура
- Применение принципа CQRS

## Технологии

- **Frontend**: React
- **Backend**: Node.js, .NET Core
- **Database**: PostgreSQL
- **Message Broker**: RabbitMQ
- **Search Engine**: BsaSearch
- **OS**: Astra Linux 1.7, РЕД ОС 7.3

## Установка

Требования к окружению:
- РЕД ОС 7.3

### Главный экран
![Main Screen](./rui.png)

Убедиться что соединение с сетью интернет стабильно.

- сделать исполняемым скрипт `install.sh` командой `chmod +x install.sh`
- запустить скрипт командой `sudo ./install.sh`
- на запрос пароля для `sudo` ввести его

Если требуется поддержка HTTPS, перед установкой скопируйте crt и key файлы в папку sslcert. Имя файла должно содержать название домена и расширение.
Например, для домена yandex.ru имена файлов долдны быть yandex.ru.crt и yandex.ru.key.
Настройка HTTPS в процессе доработки

В процессе установки требуется ввод пользователя:

Если требуется выполнить установку без доступа к сети, то в диалоге
![OFFLINE?](./roffline.png)

- R7-Disk Installer "Make OFFLINE instalation?": Выбрать Да (Yes)
Если требуется выполнить установку с доступом к сети, то в диалоге
- R7-Disk Installer "Make OFFLINE instalation?": Выбрать Нет (No)

  ![Site DNS](./rsitedns.png)

- cddisk "Domain :" ввести имя домена куда устанавливается cddisk *Ok* (по-умолчанию *local.r7-office.ru*)

  ![Make HTTPS?](./rhttps.png)

Если требуется выполнить установку с доступом по протоколу HTTPS, то в диалоге
- cddisk "Make HTTPS?": Выбрать Да (Yes)
Если требуется выполнить установку с доступом по протоколу HTTP, то в диалоге
- cddisk "Make HTTPS?": Выбрать Нет (No)

Дождаться завершения установки
После успешной установки, вы должны увидеть следующее сообщение:
![EndInstall?](./redos-03-endinstall1.png)

После утвердительного ответа будет произведена перезагрузка операционной системы.

   ![Reboot?](./redos-04-endinstall2.png)

После перезагрузки однократно будет запущен скрипт для проверки корректности установки.

- Будет запрошено какой домен необходимо проверить (указываются данные введенные при установке).

  ![Test domain](./redos-05-test-domain.png)

- Выполнялась ли установка с использованием HTTPS (указываются данные введенные при установке). 

  ![Test HTTPS](./redos-06-test-https.png)

- Будет произведена 30 секундная задержка для ожидания запуска сервисов.

  ![Test pause](./redos-07-test-pause.png)

- Запрос прав суперпользователя для проверки наличия лог-файлов.

  ![Test sudo](./redos-08-test-sudo.png)

- После осуществления проверки ожидается нажатие клавиши ENTER для позможности просмотра результатов проверки. 

  ![Test end](./redos-09-test-end.png)

Повторно скрипт проверки можно запустить `/opt/r7-office/r7-healthcheck.sh`.

### Подтверждение установки

После успешной установки, вы должны увидеть следующее сообщение:

Если данного сообщения нет, то скопируйте из терминала сообщения в процессе установки и сохраните в файл.
Файл загрузите на ресурс https://tmpfiles.org и отправьте

###  Как проверить какие сервисы запущены
Запущенные службы Systemd 

`systemctl —type=service —state=running`.
 
Службы запущенные через supervisord 

`sudo supervisorctl status`.

### Как проверить что запущены нужные

Команда: 
`for name in ds-metrics ds-converter ds-docservice supervisord postgresql rabbitmq-server redis nginx; do echo ${name} $(systemctl is-active ${name}) $(systemctl is-enabled ${name}); done | column -t | grep --color=always '\(disabled\|inactive\|$\)'`

Результат:

```
ds-metrics       active  enabled
ds-converter     active  enabled
ds-docservice    active  enabled
supervisord      active  enabled
postgresql       active  enabled
rabbitmq-server  active  enabled
redis            active  enabled
nginx            active  enabled
```

Важен статус `active`

Подробное состояние можно получить командой `systemctl status <имя сервиса>`

Команда: 
`sudo supervisorctl status "cddisk:*"`

Результат
```
cddisk:api                       RUNNING   pid 1213, uptime 0:45:54
cddisk:filestorage               RUNNING   pid 1214, uptime 0:45:54
cddisk:processing                RUNNING   pid 1215, uptime 0:45:54
cddisk:registry                  RUNNING   pid 1217, uptime 0:45:54
cddisk:searchapi                 RUNNING   pid 1216, uptime 0:45:54
```

Важен статус `RUNNING`

### Как проверить логи сервисов

Логи сервисов расположены:

- Сервера документов /var/log/r7-office/documentserver
- CDDisk /var/log/r7-office/CDDisk
- nginx /var/log/nginx
- rabbitmq /var/log/rabbitmq
- redis /var/log/redis
- supervisor /var/log/supervisor

## Контакты

Если у вас есть вопросы или предложения, пожалуйста, свяжитесь с нами через телеграм @lex_ufa.
