---
title: "Размещение ASP.NET Core в операционной системе Linux с Apache"
description: "Дополнительные сведения о настроить Apache как обратного прокси-сервера на CentOS для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС Kestrel."
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 61827f456ba01ffa726f3446401156409b29111d
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Размещение ASP.NET Core в операционной системе Linux с Apache

Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)

С помощью этого руководства, узнайте, как настроить [Apache](https://httpd.apache.org/) в качестве обратного прокси-сервера на [CentOS 7](https://www.centos.org/) для перенаправления трафика HTTP к веб-приложению ASP.NET Core ОС [Kestrel](xref:fundamentals/servers/kestrel). [Mod_proxy расширения](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) и связанных модулей создание обратного прокси-сервер.

## <a name="prerequisites"></a>Предварительные требования

1. Сервер под управлением CentOS 7 с помощью учетной записи обычного пользователя с правами sudo
2. Приложения ASP.NET Core

## <a name="publish-the-app"></a>Публикация приложения

Опубликуйте приложение в качестве [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd) в конфигурации выпуска для среды выполнения CentOS 7 (`centos.7-x64`). Скопируйте содержимое *bin/Release/netcoreapp2.0/centos.7-x64/publish* папки на сервере с помощью точки подключения службы, FTP или другой метод передачи файлов.

> [!NOTE]
> В рабочих сценариях развертывания непрерывной интеграции рабочего процесса выполняет работу, публикации приложения и копировании ресурсов на сервере. 

## <a name="configure-a-proxy-server"></a>Настройка прокси-сервера

Обратный прокси-сервер общих настроен для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запроса и отправляет его в приложение ASP.NET.

Прокси-сервер, называется источник, который перенаправляет запросы клиента на другой сервер вместо самого выполнении запроса. Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов. В этом руководстве Apache настраивается как обратный прокси-сервер запущен на том же сервере, что Kestrel обслуживает приложение ASP.NET Core.

Поскольку запросы перенаправляются обратного прокси-сервера, с помощью перенаправленных заголовки по промежуточного слоя из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) пакета. Обновления по промежуточного слоя `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовок, чтобы правильно работать, идентификаторы URI перенаправления и другие политики безопасности.

При использовании любого типа проверки подлинности по промежуточного слоя, необходимо запустить сначала по промежуточного слоя перенаправленных заголовки. Этот гарантирует, что по промежуточного слоя проверки подлинности можно использовать значения заголовка и создавать правильный перенаправления идентификаторы URI.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) и [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или аналогичные схему проверки подлинности по промежуточного слоя:

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

Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) для указания по промежуточного слоя, заголовки по умолчанию для пересылки `None`.

### <a name="install-apache"></a>Установка Apache

Пакеты обновлений CentOS их последней стабильной версии:

```bash
sudo yum update -y
```

Установить веб-сервер Apache на CentOS с одним `yum` команды:

```bash
sudo yum -y install httpd mod_ssl
```

Пример выходных данных после выполнения команды.

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> В этом примере выходных данных отражает httpd.86_64, так как 64-разрядные версии CentOS 7. Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки. 

### <a name="configure-apache-for-reverse-proxy"></a>Настройка Apache в качестве обратного прокси-сервера

Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`. Все файлы с *.conf* расширения обрабатывается в алфавитном порядке, кроме файлов конфигурации модуля в `/etc/httpd/conf.modules.d/`, содержащий какой-либо настройки файлы для загрузки модулей.

Создайте файл конфигурации для приложения с именем `hellomvc.conf`:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

**VirtualHost** узел может отображаться несколько раз в один или несколько файлов на сервере. **VirtualHost** равно прослушивать все IP-адрес, который использует порт 80. Следующие две строки, задаются на запросы прокси-сервера в корневом каталоге, чтобы на сервер по адресу 127.0.0.1 на порт 5000. Для двусторонней связи *ProxyPass* и *ProxyPassReverse* являются обязательными.

Можно настроить ведение журнала для каждого **VirtualHost** с помощью **ErrorLog** и **CustomLog** директивы. **ErrorLog** — это расположение, где сервер регистрирует ошибки, и **CustomLog** задает имя файла и формат файла журнала. В этом случае это где записываются сведения запроса. Имеется одна строка для каждого запроса.

Сохраните файл и протестировать конфигурацию. Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Перезапустите Apache:

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Мониторинг приложений

Apache сейчас программу установки, чтобы перенаправлять запросы, адресованные `http://localhost:80` для ASP.NET Core приложение, работающее на Kestrel на `http://127.0.0.1:5000`.  Однако Apache настроена для управления процессом Kestrel. Используйте *systemd* и создайте файл службы для запуска и наблюдения за базовой веб-приложения. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 


### <a name="create-the-service-file"></a>Создание файла службы

Создайте файл определения службы.

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Пример файла службы для приложения:

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **Пользователь** &mdash; Если пользователь *apache* не используется в конфигурации пользователь должен сначала создать и получает соответствующие права владения для файлов.

Сохраните файл и включите службы:

```bash
systemctl enable kestrel-hellomvc.service
```

Запустите службу и проверьте, работает ли:

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Обратный прокси-сервер настроен и Kestrel, управляемых с помощью *systemd*, веб-приложения, полностью настроена и может осуществляться из браузера на локальном компьютере в `http://localhost`. Проверка заголовки ответа **сервера** заголовок указывает, что приложения ASP.NET Core обслуживается Kestrel:

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Поскольку веб-приложения с помощью Kestrel управляется с помощью *systemd*, для централизованного журнала регистрируются события и процессов. Тем не менее, этот журнал содержит записи для всех служб и процессов, управляемых *systemd*. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Для фильтрации времени, укажите параметры времени с помощью команды. Например, использовать `--since today` для фильтрации в настоящее время или `--until 1 hour ago` для просмотра операций предыдущий час. Дополнительные сведения см. в разделе [для получения journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Обеспечение безопасности приложения

### <a name="configure-firewall"></a>Настройка брандмауэра

*Firewalld* — динамический управляющая программа для управления брандмауэром с поддержкой зон сети. Порты и фильтрации пакетов можно по-прежнему осуществлять утилита iptables. *Firewalld* должен быть установлен по умолчанию. `yum`можно использовать для установки пакета или убедитесь, что она установлена.

```bash
sudo yum install firewalld -y
```

Используйте `firewalld` открыть только порты, необходимые для приложения. В этом случае используются порты 80 и 443. Порты 80 и 443, чтобы открыть окончательно заданы следующие команды:

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Загрузить параметры брандмауэра. Проверьте доступные службы и порты зоны по умолчанию. Проверка кода доступны параметры `firewall-cmd -h`.

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a>Конфигурация SSL

Чтобы настроить Apache для SSL, *mod_ssl* используется модуль. Когда *httpd* модуль был установлен, *mod_ssl* модуль также был установлен. Если он не был установлен, используйте `yum` Чтобы добавить его в конфигурацию.

```bash
sudo yum install mod_ssl
```
Чтобы принудительно использовать SSL, установите `mod_rewrite` для включения перезаписи URL-адресов:

```bash
sudo yum install mod_rewrite
```

Изменить *hellomvc.conf* файл, чтобы включить перезаписи URL-адресов и безопасный обмен данными через порт 443:

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> В этом примере используется локально создаваемого сертификата. **SSLCertificateFile** должен быть файлом основного сертификата для доменного имени. **SSLCertificateKeyFile** файл ключа генерацию при создании CSR. **SSLCertificateChainFile** должен быть файл промежуточного сертификата (если таковые имеются), предоставленный центром сертификации.

Сохраните файл и проверьте правильность конфигурации:

```bash
sudo service httpd configtest
```

Перезапустите Apache:

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Дополнительные предложения Apache

### <a name="additional-headers"></a>Дополнительные заголовки

Для защиты от вредоносных атак, существует несколько заголовков, которые следует быть изменены или добавлены. Убедитесь, что `mod_headers` модуль установлен:

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Защита от атак clickjacking Apache

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), также известных как *пользовательского интерфейса redress атаки*, является вредоносной атаки, где браузера обманным путем щелчок ссылки или кнопки на другую страницу, не посещается в данный момент. Используйте `X-FRAME-OPTIONS` для защиты узла.

Изменить *httpd.conf* файла:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Сохраните файл. Перезапустите Apache.

#### <a name="mime-type-sniffing"></a>Сканирование типа MIME

`X-Content-Type-Options` Заголовок предотвращает Internet Explorer из *сканирование MIME* (определение файла `Content-Type` из содержимого файла). Если сервер задает `Content-Type` заголовок `text/html` с `nosniff` набор параметров, Internet Explorer отображает содержимое как `text/html` независимо от его содержимого.

Изменить *httpd.conf* файла:

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header set X-Content-Type-Options "nosniff"`. Сохраните файл. Перезапустите Apache.

### <a name="load-balancing"></a>Балансировка нагрузки 

В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере. Чтобы не имеют одной точки сбоя; с помощью *mod_proxy_balancer* и изменение **VirtualHost** позволит для управления экземплярами несколько веб-приложений за прокси-сервера Apache.

```bash
sudo yum install mod_proxy_balancer
```

В файле конфигурации показано ниже, дополнительный экземпляр `hellomvc` приложения настроен для запуска на порту 5001. *Прокси* раздел, задайте для балансировки нагрузки с конфигурацией подсистемы балансировки с двумя членами *byrequests*.

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a>Ограничения скорости
С помощью *mod_ratelimit*, который входит в *httpd* модуля, пропускную способность для клиентов может быть ограничен:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
Пример файла ограничивает пропускную способность как 600 КБ в секунду в корневой папке:

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
