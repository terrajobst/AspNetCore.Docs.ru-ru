---
title: "Среда размещения ASP.NET Core в операционной системе Linux с Nginx"
description: "Описание процедуры настройки Nginx в качестве обратного прокси-сервера на 16.04 Ubuntu для пересылки трафика HTTP в веб-приложение ASP.NET Core ОС Kestrel."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 08/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 465f1391ef4ff9492d9aed48cb32da0659ceda41
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Среда размещения ASP.NET Core в операционной системе Linux с Nginx

Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)

В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере 16.04 Ubuntu.

**Примечание:** для Ubuntu 14.04 *supervisord* рекомендуется в качестве решения для мониторинга процесса Kestrel. *systemd* не доступен на Ubuntu 14.04. [См. предыдущую версию этого документа](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

В этом руководстве рассматривается

* Помещает существующего приложения ASP.NET Core за обратного прокси-сервера.
* Настраивает обратного прокси-сервера для перенаправления запросов на Kestrel веб-сервера.
* Гарантирует, что веб-приложение запускается при запуске как управляющая программа.
* Настраивает средство управления процессами, чтобы перезапустить веб-приложение.

## <a name="prerequisites"></a>Предварительные требования

1. Доступ к серверу Ubuntu 16.04 под учетной записи обычного пользователя с правами sudo
1. Существующие приложения ASP.NET Core

## <a name="copy-over-the-app"></a>Скопировать приложение

Выполните из среды разработки команду `dotnet publish`, чтобы упаковать приложение в отдельный каталог, который можно запустить на сервере.

Скопируйте приложения ASP.NET Core на сервер с помощью того средства, интегрируется в рабочий процесс в организации (например, точка подключения службы, FTP). Протестируйте приложение, например

* Запустите из командной строки, `dotnet <app_assembly>.dll`.
* В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение работает на платформе Linux. 
 
## <a name="configure-a-reverse-proxy-server"></a>Настройка обратного прокси-сервера

Обратный прокси-сервер общих настроен для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запроса и отправляет его в приложение ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Для чего нужен обратный прокси-сервер?

Kestrel превосходно справляется с обслуживанием динамического содержимого из ASP.NET Core; при этом компоненты для работы с веб-службами не настолько функциональны, как серверы типа IIS, Apache или Nginx. Обратный прокси-сервер может облегчить такую работу, как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершение SSL с HTTP-сервера. Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.

В контексте данного руководства используется отдельный экземпляр Nginx. Он выполняется на том же сервере, что и HTTP-сервер. В соответствии с требованиями различных установка может быть выбираться.

Так как запросы перенаправляются обратным прокси-сервером, используйте ПО промежуточного слоя `ForwardedHeaders` из пакета `Microsoft.AspNetCore.HttpOverrides`. Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.

При настройке обратного прокси-сервера для промежуточного ПО проверки подлинности необходимо в первую очередь выполнить команду `UseForwardedHeaders`. Такой порядок гарантирует, что промежуточное ПО проверки подлинности получит соответствующие значения и сформирует верные URI перенаправления.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Вызовите метод `UseForwardedHeaders` (в методе `Configure` файла *Startup.cs*) перед вызовом `UseAuthentication` или другого ПО промежуточного слоя, предназначенного для проверки подлинности.

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Вызовите метод `UseForwardedHeaders` (в методе `Configure` файла *Startup.cs*) перед вызовом `UseIdentity` и `UseFacebookAuthentication` или другого ПО промежуточного слоя, предназначенного для проверки подлинности.

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a>Установка Nginx

```bash
sudo apt-get install nginx
```

> [!NOTE]
> Если дополнительных модулей Nginx будут установлены, может потребоваться создание Nginx из источника.

Установите Nginx с помощью команды `apt-get`. Программа установки создает сценарий инициализации System V, который запускает Nginx как управляющую программу Nginx при запуске системы. Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.

```bash
sudo service nginx start
```

В браузере должна открыться стартовая страница Nginx по умолчанию.

### <a name="configure-nginx"></a>Настройка Nginx

Чтобы настроить в качестве обратного прокси-сервера для пересылки запросов нашего приложения ASP.NET Core Nginx, измените `/etc/nginx/sites-available/default`. Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.

```
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Этот файл конфигурации Nginx перенаправляет входящий публичный трафик с порта `80` на порт `5000`.

После установления Nginx конфигурации запуска `sudo nginx -t` проверить синтаксис файлов конфигурации. Если проверка файла конфигурации прошла успешно, принудительно Nginx, чтобы принять изменения, запустив `sudo nginx -s reload`.

## <a name="monitoring-the-app"></a>Мониторинг приложений

Сервер настроен для пересылки запросов к `http://<serveraddress>:80` на ASP.NET Core приложение, работающее на Kestrel на `http://127.0.0.1:5000`. Однако Nginx настроена для управления процессом Kestrel. *systemd* можно использовать для создания файла службы для запуска и наблюдения за базовой веб-приложения. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 

### <a name="create-the-service-file"></a>Создание файла службы

Создайте файл определения службы.

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Ниже приведен пример файла службы для приложения.

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

**Примечание:** Если пользователь *www данных* не используется в конфигурации пользовательские необходимо сначала создать и получает соответствующие права владения для файлов.
**Примечание:** Linux содержит с учетом регистра файловую систему. Параметр ASPNETCORE_ENVIRONMENT поиск файла конфигурации приводит к «Production» *appsettings. Production.JSON*, а не *appsettings.production.json*.

Сохраните файл и включите службу.

```bash
systemctl enable kestrel-hellomvc.service
```

Запустите службу и убедитесь, что он работает.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Обратный прокси-сервер настроен и управляемых с помощью systemd Kestrel, веб-приложения, полностью настроена и может осуществляться из браузера на локальном компьютере, на `http://localhost`. Он также доступен с удаленного компьютера, за исключением выполнения любой брандмауэр может блокировать. Проверка заголовки ответа `Server` заголовок показывает предоставляемый Kestrel приложения ASP.NET Core.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Поскольку веб-приложения с помощью Kestrel управляется с помощью `systemd`, для централизованного журнала регистрируются все события и процессов. При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Обеспечение безопасности приложения

### <a name="enable-apparmor"></a>Включение AppArmor

Модули безопасности Linux (LSM) — это платформа, который является частью ядро Linux с момента Linux 2.6. LSM поддерживают различные реализации модулей безопасности. [AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов. Убедитесь, что AppArmor включен и правильно настроен.

### <a name="configuring-the-firewall"></a>Настройка брандмауэра

Закройте все внешние порты, которые не используются. Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана. Убедитесь, что `ufw` настроен для разрешения трафика на все необходимые порты.

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Защита Nginx

В дистрибутиве Nginx по умолчанию SSL не включен. Чтобы включить дополнительные функции безопасности, выполните сборку из исходного файла.

#### <a name="download-the-source-and-install-the-build-dependencies"></a>Загрузка исходного файла и установка зависимостей сборки

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Изменение имени ответа Nginx

Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Настройка параметров и сборки

Для регулярных выражений требуется библиотека PCRE. Регулярные выражения используются в директиве местонахождения для модуля ngx_http_rewrite_module. Модуль http_ssl_module добавляет поддержку протокола HTTPS.

Рассмотрите возможность использования брандмауэра web app как *ModSecurity* для усиления защиты приложения.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Настройка SSL

* Настройка сервера для прослушивания трафика HTTPS на порте `443` , указав действительный сертификат, выпущенный доверенного центра сертификации (ЦС).

* Улучшение безопасности при использовании некоторые рекомендации, приведенные в следующей */etc/nginx/nginx.conf* файла. Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.

* При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.

* Не добавляйте в заголовке безопасности для транспорта Strict или с соответствующей `max-age` Если SSL будет отключена в будущем.

Добавьте файл конфигурации */etc/nginx/proxy.conf*:

[!code-nginx[Main](linux-nginx/proxy.conf)]

Измените файл конфигурации */etc/nginx/proxy.conf*. В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.

[!code-nginx[Main](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Защита Nginx от кликджекинга
Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя. Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты. Использование X-FRAME-OPTIONS для защиты узла.

Измените файл *nginx.conf*.

```bash
sudo nano /etc/nginx/nginx.conf
```

Добавьте строку `add_header X-Frame-Options "SAMEORIGIN";` и сохраните файл, а затем перезапустите Nginx.

#### <a name="mime-type-sniffing"></a>Сканирование типа MIME

Этот заголовок предотвращает MIME-сканирование ответов с указанным типом содержимого в большинстве браузеров, запрещая браузеру переопределять тип содержимого ответа. Параметр `nosniff` означает, что, если сервер определяет содержимое как text/html, браузер будет обрабатывать его как text/html.

Измените файл *nginx.conf*.

```bash
sudo nano /etc/nginx/nginx.conf
```

Добавьте строку `add_header X-Content-Type-Options "nosniff";` и сохраните файл, а затем перезапустите Nginx.
