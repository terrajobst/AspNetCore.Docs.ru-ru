---
title: "Среда размещения ASP.NET Core в операционной системе Linux с Nginx"
description: "В статье описывается процедура настройки Nginx как обратного прокси-сервера на Ubuntu 16.04 для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel."
keywords: "ASP.NET Core, Linux, nginx, Ubuntu, обратный прокси-сервер"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 01768263fe82dc75a7da0e113b1850c8d788bfd3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Настройка среды размещения для ASP.NET Core в операционной системе Linux с Nginx и развертывание в эту среду

Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)

В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере 16.04 Ubuntu.

**Примечание**. Для Ubuntu 14.04 в качестве решения для мониторинга процесса Kestrel рекомендуется supervisord. Решение systemd в Ubuntu 14.04 недоступно. [См. предыдущую версию этого документа](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

В этом руководстве рассматривается

* Размещение существующего приложения ASP.NET Core за обратным прокси-сервером
* Настройка обратного прокси-сервера для перенаправления запросов на веб-сервер Kestrel
* Проверка выполнения веб-приложения как управляющей программы при запуске системы
* Настройка средства управления процессами в помощь с перезапуском веб-приложения

## <a name="prerequisites"></a>Предварительные требования

1. Доступ к серверу Ubuntu 16.04 под учетной записи обычного пользователя с правами sudo
2. Существующее приложение ASP.NET Core

## <a name="copy-over-your-app"></a>Копирование приложения

Выполните из среды разработки команду `dotnet publish`, чтобы упаковать приложение в отдельный каталог, который можно запустить на сервере.

Скопируйте приложение ASP.NET Core на сервер с помощью инструмента (SCP, FTP и т. д.), интегрированного в ваш рабочий процесс. Протестируйте приложение, например
 - Выполните из командной строки команду `dotnet yourapp.dll`.
 - В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение работает на платформе Linux. 
 
**Примечание**. Для создания нового приложения ASP.NET Core для нового проекта используйте [Yeoman](xref:client-side/yeoman).

## <a name="configure-a-reverse-proxy-server"></a>Настройка обратного прокси-сервера

Обратный прокси-сервер — это общая настройка для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.

### <a name="why-use-a-reverse-proxy-server"></a>Для чего нужен обратный прокси-сервер?

Kestrel превосходно справляется с обслуживанием динамического содержимого из ASP.NET Core; при этом компоненты для работы с веб-службами не настолько функциональны, как серверы типа IIS, Apache или Nginx. Обратный прокси-сервер может облегчить такую работу, как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершение SSL с HTTP-сервера. Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.

В контексте данного руководства используется отдельный экземпляр Nginx. Он выполняется на том же сервере, что и HTTP-сервер. Вы можете выбрать другую конфигурацию в зависимости от своих условий.

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
> Для установки дополнительных модулей Nginx может потребоваться сборка Nginx из источника.

Установите Nginx с помощью команды `apt-get`. Программа установки создает сценарий инициализации System V, который запускает Nginx как управляющую программу Nginx при запуске системы. Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.

```bash
sudo service nginx start
```

В браузере должна открыться стартовая страница Nginx по умолчанию.

### <a name="configure-nginx"></a>Настройка Nginx

Чтобы настроить Nginx как обратный прокси-сервер для перенаправления запросов в наше приложение ASP.NET Core, измените файл `/etc/nginx/sites-available/default`. Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Этот файл конфигурации Nginx перенаправляет входящий публичный трафик с порта `80` на порт `5000`.

Закончив внесение изменений в конфигурацию Nginx, выполните команду `sudo nginx -t`, чтобы проверить синтаксис файлов конфигурации. Если проверка файла конфигурации прошла успешно, укажите Nginx, что нужно принять изменения, выполнив команду `sudo nginx -s reload`.

## <a name="monitoring-our-application"></a>Мониторинг приложения

Теперь Nginx настроен на перенаправление запросов, отправленных в `http://yourhost:80`, в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`. При этом Nginx не настроен для управления процессом Kestrel. Для запуска и мониторинга соответствующего веб-приложения можно воспользоваться *systemd* и создать файл службы. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 

### <a name="create-the-service-file"></a>Создание файла службы

Создайте файл определения службы.

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Пример файла службы для нашего приложения.

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

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

**Примечание**. Если пользователь *www-data* в вашей конфигурации не используется, необходимо сначала создать определенного выше пользователя, а затем предоставить ему необходимые права владения для файлов.

Сохраните файл и включите службу.

```bash
systemctl enable kestrel-hellomvc.service
```

Запустите службу и убедитесь, что она работает.

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Теперь, когда обратный прокси-сервер настроен, а управление Kestrel осуществляется системой systemd, веб-приложение полностью настроено и готово для доступа из браузера на локальном компьютере по адресу `http://localhost`. Оно также доступно для удаленных компьютеров, несмотря на наличие блокирующих трафик межсетевых экранов. Если в ответах есть заголовок `Server`, это означает, что приложение ASP.NET Core обслуживает Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Так как веб-приложение, использующее Kestrel, управляется с помощью `systemd`, все события и процессы регистрируются в централизованном журнале. При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>Защита приложения

### <a name="enable-apparmor"></a>Включение AppArmor

Начиная с версии 2.6, ядро Linux включает платформу модулей безопасности Linux (LSM). LSM поддерживают различные реализации модулей безопасности. [AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов. Убедитесь, что AppArmor включен и правильно настроен.

### <a name="configuring-our-firewall"></a>Настройка межсетевого экрана

Закройте все внешние порты, которые не используются. Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана. Убедитесь, что настройки `ufw` пропускают трафик на все необходимые порты.

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

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Изменение имени ответа Nginx

Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>Настройка параметров и сборки

Для регулярных выражений требуется библиотека PCRE. Регулярные выражения используются в директиве местонахождения для модуля ngx_http_rewrite_module. Модуль http_ssl_module добавляет поддержку протокола HTTPS.

Для дополнительной защиты приложения можно использовать межсетевой экран для веб-приложений, такой как *ModSecurity*.

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>Настройка SSL

* Настройте сервер для прослушивания трафика HTTPS через порт `443`, указав действительный сертификат, выпущенный доверенным центром сертификации (ЦС).

* Укрепите защиту, применив некоторые методы, показанные в представленном ниже файле */etc/nginx/nginx.conf*. Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.

* При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.

* Если в дальнейшем вы планируете отключить SSL, не добавляйте заголовок Strict-Transport-Security или выберите соответствующий `max-age`.

Добавьте файл конфигурации */etc/nginx/proxy.conf*:

[!code-nginx[Main](linuxproduction/proxy.conf)]

Измените файл конфигурации */etc/nginx/proxy.conf*. В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Защита Nginx от кликджекинга
Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя. Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты. Для защиты своего сайта используйте X-FRAME-OPTIONS.

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
