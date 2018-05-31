---
title: Размещение ASP.NET Core в операционной системе Linux с Apache
description: Процедура настройки Apache в качестве обратного прокси-сервера в CentOS для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel.
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962821"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Размещение ASP.NET Core в операционной системе Linux с Apache

Автор: [Шейн Бойер (Shayne Boyer)](https://github.com/spboyer)

Из этого руководства вы узнаете, как настроить [Apache](https://httpd.apache.org/) в качестве обратного прокси-сервера в [CentOS 7](https://www.centos.org/) для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в [Kestrel](xref:fundamentals/servers/kestrel). [Расширение mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) и связанные с ним модули создают обратный прокси-сервер.

## <a name="prerequisites"></a>Предварительные требования

1. Сервер под управлением CentOS 7 и учетная запись обычного пользователя с правами sudo.
2. Приложение ASP.NET Core.

## <a name="publish-the-app"></a>Публикация приложения

Опубликуйте приложение в качестве [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в конфигурации выпуска для среды выполнения CentOS 7 (`centos.7-x64`). Скопируйте содержимое папки *bin/Release/netcoreapp2.0/centos.7-x64/publish* на сервер с помощью SCP, FTP или другого метода передачи файлов.

> [!NOTE]
> Если развертывание выполняется в рабочей среде, рабочий процесс непрерывной интеграции автоматически опубликует приложение и скопирует его ресурсы на сервер. 

## <a name="configure-a-proxy-server"></a>Настройка прокси-сервера

Обратный прокси-сервер — это стандартный вариант настройки для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET.

Прокси-сервер перенаправляет запросы клиента на другой сервер, а не обрабатывает их самостоятельно. Обратный прокси-сервер перенаправляет запросы в фиксированное назначение обычно от имени клиентов. При работе с этим руководством мы настроим Apache в качестве обратного прокси-сервера, который работает на том же сервере, где Kestrel предоставляет приложение ASP.NET Core.

Так как запросы перенаправляются обратным прокси-сервером, используйте ПО промежуточного слоя перенаправления заголовков, входящее в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.

Вне зависимости от того, какой тип ПО промежуточного слоя используется для проверки подлинности, сначала необходимо запустить ПО промежуточного слоя перенаправления заголовков. Такой порядок гарантирует, что ПО промежуточного слоя для проверки подлинности может использовать значения заголовков и сформирует правильные URI перенаправления.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызывать [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме. В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызывать [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity), [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме. В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:

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

Если параметр [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) не задан для ПО промежуточного слоя, по умолчанию перенаправляются заголовки `None`.

Для приложений, размещенных за прокси-серверами и подсистемами балансировки нагрузки, может потребоваться дополнительная настройка. Дополнительные сведения см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-apache"></a>Установка Apache

Обновите пакеты CentOS до последних стабильных версий:

```bash
sudo yum update -y
```

Установите веб-сервер Apache на CentOS, выполнив одну команду `yum`:

```bash
sudo yum -y install httpd mod_ssl
```

Пример выходных данных этой команды:

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
> В нашем примере выходные данные содержат строку httpd.86_64, так как используется 64-разрядная версия CentOS 7. Чтобы проверить, где установлен Apache, выполните `whereis httpd` из командной строки.

### <a name="configure-apache-for-reverse-proxy"></a>Настройка Apache в качестве обратного прокси-сервера

Файлы конфигурации для Apache находятся в каталоге `/etc/httpd/conf.d/`. В алфавитном порядке обрабатываются все файлы с расширением *.conf*, а также файлы конфигурации модуля из папки `/etc/httpd/conf.modules.d/`, где содержатся файлы конфигурации, необходимые для загрузки модулей.

Создайте для приложения файл конфигурации с именем *hellomvc.conf*:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

Блок `VirtualHost` может встречаться несколько раз в одном или нескольких файлах на сервере. С представленным выше файлом конфигурации Apache принимает трафик от любого источника через порт 80. Он обслуживает домен `www.example.com`, а псевдоним `*.example.com` указывает на тот же веб-сайт. Дополнительные сведения см. в статье [о поддержке виртуальных узлов на основе имен](https://httpd.apache.org/docs/current/vhosts/name-based.html). Запросы к корневому каталогу перенаправляются на порт 5000 того же сервера по адресу 127.0.0.1. Для двусторонней связи требуются `ProxyPass` и `ProxyPassReverse`.

> [!WARNING]
> Если не удастся указать правильную [директиву ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) в блоке **VirtualHost**, приложение будет иметь значительные уязвимости. Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.example.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость). Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

Ведение журнала можно настроить отдельно для каждого `VirtualHost` с помощью директив `ErrorLog` и `CustomLog`. Журналы ошибок сохраняются в расположение `ErrorLog`, а параметр `CustomLog` задает имя и формат для файла журнала. В нашем примере здесь фиксируются сведения о запросах. Для каждого запроса создается одна строка.

Сохраните файл и протестируйте конфигурацию. Если проверка выполнена успешно, ответ должен быть `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Перезапустите Apache.

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Мониторинг приложения

Теперь Apache настроен на перенаправление запросов к `http://localhost:80` в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`.  Но Apache не настроен для управления процессом Kestrel. Для запуска и мониторинга базового веб-приложения используйте *systemd* и создайте файл службы. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 


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
> **Пользователь.** Если пользователь *apache* в вашей конфигурации еще не используется, его необходимо создать и правильно предоставить ему права владения для соответствующих файлов.

> [!NOTE]
> Некоторые значения (например, строки подключения SQL) необходимо экранировать, чтобы поставщики конфигурации могли считать переменные среды. Используйте следующую команду, чтобы создать правильно экранированное значение для файла конфигурации:
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Сохраните файл и включите службу.

```bash
systemctl enable kestrel-hellomvc.service
```

Запустите службу и убедитесь, что она работает.

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

Теперь, когда обратный прокси-сервер настроен и *systemd* управляет процессом Kestrel, веб-приложение можно считать полностью настроенным и вы можете обратиться к нему по адресу `http://localhost` из браузера на локальном компьютере. Заголовок **Server** в ответе подтверждает, что приложение ASP.NET Core обслуживается Kestrel.

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Так как веб-приложением, использующим Kestrel, управляет *systemd*, все события и процессы регистрируются в централизованном журнале. Но этот журнал содержит все записи обо всех службах и процессах, управляемых *systemd*. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Чтобы отфильтровать элементы по времени, укажите в команде параметры времени. Например, `--since today` позволяет отфильтровать данные за текущий день, а `--until 1 hour ago` — просмотреть записи за предыдущий час. Дополнительные сведения см. на странице руководства о команде [journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Защита приложения

### <a name="configure-firewall"></a>Настройка брандмауэра

*Firewalld* — это динамическая управляющая программа брандмауэра, которая поддерживает зоны сети. Фильтрацию портов и пакетов можно по-прежнему осуществлять через iptables. *Firewalld* обычно устанавливается в системе по умолчанию. `yum` позволяет установить этот пакет или убедиться, что он установлен.

```bash
sudo yum install firewalld -y
```

С помощью `firewalld` вы можете открыть только те порты, которые необходимые для работы приложения. В этом случае используются порты 80 и 443. Следующие команды назначают порты 80 и 443 постоянно открытыми.

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Обновите параметры брандмауэра. Проверьте, что доступные службы и порты находятся в зоне по умолчанию. Эти параметры можно просмотреть с помощью `firewall-cmd -h`.

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

Apache для SSL настраивается с помощью модуля *mod_ssl*. При установке модуля *httpd* автоматически добавляется и модуль *mod_ssl*. Если он по каким-то причинам отсутствует, добавьте его в систему с помощью `yum`.

```bash
sudo yum install mod_ssl
```
Чтобы принудительно использовать SSL, установите модуль `mod_rewrite` для перезаписи URL-адресов.

```bash
sudo yum install mod_rewrite
```

Измените файл *hellomvc.conf*, чтобы разрешить перезапись URL-адресов и безопасный обмен данными через порт 443.

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
> В этом примере используется локально созданный сертификат. В **SSLCertificateFile** должен быть указан основной файл сертификата для доменного имени. В **SSLCertificateKeyFile** должен быть указан файл ключа, сформированный при создании CSR. В **SSLCertificateChainFile** должен быть указан файл промежуточного сертификата (если он существует), предоставленный центром сертификации.

Сохраните файл и протестируйте конфигурацию.

```bash
sudo service httpd configtest
```

Перезапустите Apache.

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Дополнительные предложения Apache

### <a name="additional-headers"></a>Дополнительные заголовки

Для защиты от вредоносных атак следует изменить или добавить несколько заголовков. Убедитесь, что модуль `mod_headers` установлен.

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Защита Apache от атак кликджекинга

[Кликджекинг](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger) (или *атака с подменой пользовательского интерфейса*) является вредоносной атакой, при которой посетителя сайта обманным путем вынуждают щелкнуть ссылку или нажать кнопку не той страницы, на которой он находится. Используйте `X-FRAME-OPTIONS` для защиты сайта.

Измените файл *httpd.conf*.

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Сохраните файл. Перезапустите Apache.

#### <a name="mime-type-sniffing"></a>Сканирование типа MIME

Заголовок `X-Content-Type-Options` защищает Internet Explorer от *сканирования MIME* (определения `Content-Type` для файла по его содержимому). Если сервер задает заголовок `Content-Type` со значением `text/html`, и при этом установлен параметр `nosniff`, Internet Explorer отображает содержимое как `text/html` независимо от содержимого файла.

Измените файл *httpd.conf*.

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Добавьте строку `Header set X-Content-Type-Options "nosniff"`. Сохраните файл. Перезапустите Apache.

### <a name="load-balancing"></a>Балансировка нагрузки 

В этом примере показано, как установить и настроить Apache в CentOS 7 и Kestrel на том же компьютере. Чтобы устранить единую точку отказа, воспользуйтесь *mod_proxy_balancer* и измените значение **VirtualHost** для управления несколькими экземплярами веб-приложений за прокси-сервером Apache.

```bash
sudo yum install mod_proxy_balancer
```

В представленном ниже файле конфигурации настроен дополнительный экземпляр приложения `hellomvc`, работающий на порту 5001. В разделе *Proxy* настраивается конфигурация подсистемы балансировки нагрузки с двумя членами для распределения нагрузки методом *byrequests*.

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
С помощью элемента *mod_ratelimit*, который входит в модуль *httpd*, можно ограничить пропускную способность для клиентов:

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
В примере файла пропускная способность составляет 600 Кбит/с в корневой папке.

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
