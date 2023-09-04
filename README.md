# RServer Astra Linux
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
- Astra Linux 1.7 "Смоленск"

### Main Screen
![Main Screen](./main.png)


Убедиться что соединение с сетью интернет стабильно.

- сделать исполняемым скрипт `install-web.sh` командой `chmod +x install-web.sh`
- запустить скрипт командой `./install-web.sh`
- на запрос пароля для `sudo` ввести его

Если требуется поддержка HTTPS, перед установкой скопируйте crt и key файлы в папку sslcert. Имя файла должно содержать название домена и расширение.
Например, для домена yandex.ru имена файлов долдны быть yandex.ru.crt и yandex.ru.key.

В процессе установки требуется ввод пользователя:
Если требуется выполнить чистую установку, то в диалоге
- Настраивается cddisk "Make clean install?": Выбрать Да (Yes)
Если не требуется выполнить чистую установку, то в диалоге
- Настраивается cddisk "Make clean install?": Выбрать Нет (No)
![OFFLINE?](./cleaninstall.png)
Для установки базы данных на локальный компьютер:
- Настраивается cddisk "Install postgresql on local pc?": Выбрать Да (Yes)
![OFFLINE?](./installpostgres.png)
Для установки Document Server в диалоге
- Настраивается cddisk "Install Document Server?": Выбрать Да (Yes)
![OFFLINE?](./installds.png)
- Настраивается cddisk "Enter Document Server secret:" ввести секретный ключ выбрать *Ok*
![OFFLINE?](./ds_secret.png)
- Настраивается r7-office-documentserver-ee "Database password:" ввести **saSA123$** выбрать *Ok*
![OFFLINE?](./ds_dbpasswd.png)
- Настраивается cddisk "Choose database type": Выбрать postgresql
![OFFLINE?](./postgres_type.png)
- Настраивается cddisk "Create database": Выбрать Да (Yes)
![OFFLINE?](./postgres_crdb.png)
- Настраивается cddisk "Database host": ввести имя домена куда устанавливается база данных *Ok* (по-умолчанию *localhost*)
![OFFLINE?](./postgres_host.png)
- Настраивается cddisk "Database port": ввести tcp порт базы данных *Ok* (по-умолчанию *5432*)
![OFFLINE?](./postgres_port.png)
- Настраивается cddisk "Database user for create DB": ввести имя пользователя, под которым будет работать база данных *Ok* (по-умолчанию *postgres*)
![OFFLINE?](./postgres_db.png)
- Настраивается cddisk "Database password:" два раза ввести **saSA123$** выбрать *Ok*
![OFFLINE?](./postgres_pwd.png)
![OFFLINE?](./postgres_retype_pwd.png)
- Настраивается cddisk "The salt to be used during the key derivation process:" ввести ключ, который будет использоваться для генерации хэшей паролей *Ok* (по-умолчанию *Vskoproizvolny Salt par Chivreski_*)
![OFFLINE?](./postgres_key.png)
- Настраивается cddisk "Domain name:" ввести имя домена куда устанавливается cddisk *Ok* (по-умолчанию *local.ru*)
![OFFLINE?](./cddisk_domain.png)
- Настраивается cdmail "Domain name:" ввести имя домена куда устанавливается cdmail *Ok* (по-умолчанию *local.ru*)
![OFFLINE?](./cdmail_host.png)
- Настраивается calendar "Domain name:" ввести имя домена куда устанавливается calendar *Ok* (по-умолчанию *local.ru*)
![OFFLINE?](./calendar_host.png)

Дождаться завершения установки

- Если установили https для проверки откройте https://cddisk.local.ru, если ввели домен отличный от дефолтного то https://cddisk.вашдомен, который ввели в переменную "Site domain"
- Если установили http для http://cddisk.local.ru, если ввели домен отличный от дефолтного то http://cddisk.вашдомен, который ввели в переменную "Site domain"
![OFFLINE?](./check_cddisk.png)
- Аналогично http(s)://admin.{вашдомен} http(s)://cdmail.{вашдомен} http(s)://calendar.{вашдомен}:
![OFFLINE?](./check_admin.png)
![OFFLINE?](./check_cdmail.png)
![OFFLINE?](./check_calendar.png)
- Если авторизация не работает в браузере посмотреть ответ от api нажав кнопку F12, если 502 то выполнить команду sudo supervisorctl restart cddisk:*, если при выполнение возникли ошибки, то проверить логи /var/log/r7-office/
- Если в логах EACCES то выполнить команду chmod 755 /opt/r7-office, выполнить команду sudo supervisorctl restart cddisk:*, в случае успешного старта всех сервисов выполнить запрос по url https://cddisk.вашдомен/api/v1/version

### Подтверждение установки

После успешной установки, вы должны увидеть следующее сообщение:
CDDisk setup cmpleted.

Если данного сообщения нет, то скопируйте из терминала сообщения в процессе установки и сохраните в файл.
Файл загрузите на ресурс https://tmpfiles.org и отправьте 


При локальной установке проверьте название сайтов в файле hosts:
![OFFLINE?](./check_hosts.png)


###  Как проверить какие сервисы запущены
Запущенные службы Systemd 

`systemctl —type=service —state=running`.
 
Службы запущенные через supervisord 

`sudo supervisorctl status`.

### Как проверить что запущены нужные

Команда: 
`for name in supervisord postgresql rabbitmq-server redis nginx; do echo ${name} $(systemctl is-active ${name}) $(systemctl is-enabled ${name}); done | column -t | grep --color=always '\(disabled\|inactive\|$\)'`

Результат:

```
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

Результат:
![OFFLINE?](./check_supervisor.png)

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
