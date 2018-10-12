---
title: Среда размещения ASP.NET Core в операционной системе Linux с Nginx
author: rick-anderson
description: В статье описывается процедура настройки Nginx как обратного прокси-сервера на Ubuntu 16.04 для перенаправления трафика HTTP в веб-приложение ASP.NET Core, выполняемое в Kestrel.
ms.author: riande
ms.custom: mvc
ms.date: 09/08/2018
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: bf1fb8c0db2afd4e0c6044f3d08c22d619931554
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523237"
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Среда размещения ASP.NET Core в операционной системе Linux с Nginx

Автор [Сурабх Ширхатти](https://twitter.com/sshirhatti)

В этом руководстве описывается настройка готовой к работе среды ASP.NET Core на сервере Ubuntu 16.04. Эти инструкции могут подходить для более поздних версий Ubuntu, но они еще не были протестированы в этих версиях.

Сведения о других дистрибутивах Linux, поддерживаемых платформой ASP.NET Core, см. в разделе о [необходимых компонентах для .NET Core в Linux](/dotnet/core/linux-prerequisites).

> [!NOTE]
> Для Ubuntu 14.04 в качестве решения для мониторинга процесса Kestrel рекомендуется *supervisord*. Решение *systemd* в Ubuntu 14.04 недоступно. Инструкции для Ubuntu 14.04 см. в [предыдущей версии этого раздела](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).

В этом руководстве рассматривается

* размещение существующего приложения ASP.NET Core за обратным прокси-сервером;
* настройка обратного прокси-сервера для перенаправления запросов на веб-сервер Kestrel;
* проверка выполнения веб-приложения как управляющей программы при запуске системы;
* настройка средства управления процессами, с помощью которого можно перезапустить веб-приложение.

## <a name="prerequisites"></a>Предварительные требования

1. Доступ к серверу Ubuntu 16.04 под учетной записью обычного пользователя с правами sudo.
1. Установите среду выполнения .NET Core на сервере.
   1. Перейдите на [страницу всех загрузок .NET Core](https://www.microsoft.com/net/download/all).
   1. Выберите последнюю не предварительную версию среды выполнения из списка под заголовком **Среда выполнения**.
   1. Сделайте выбор и следуйте инструкциям для Ubuntu, версия которой совпадает с версией Ubuntu на сервере.
1. Существующее приложение ASP.NET Core.

## <a name="publish-and-copy-over-the-app"></a>Публикация и копирование приложения

Настройте приложение, чтобы [его развертывание зависело от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Запустите [dotnet publish](/dotnet/core/tools/dotnet-publish) в среде разработки, чтобы упаковать приложение в каталог (например, *bin/Release/&lt;target_framework_moniker&gt;/publish*), который может выполняться на сервере:

```console
dotnet publish --configuration Release
```

Приложение может быть опубликовано как [автономное развертывание](/dotnet/core/deploying/#self-contained-deployments-scd), если вы предпочитаете не сохранять среду выполнения .NET Core на сервере.

Скопируйте приложение ASP.NET Core на сервер с помощью инструмента, интегрированного в ваш рабочий процесс (например, SCP или SFTP). Обычно веб-приложения находятся в каталоге *var* (например, *var/aspnetcore/hellomvc*).

> [!NOTE]
> Если развертывание выполняется в рабочей среде, рабочий процесс непрерывной интеграции автоматически опубликует приложение и скопирует его ресурсы на сервер.

Проверьте работу приложения:

1. Запустите приложение в командной строке: `dotnet <app_assembly>.dll`.
1. В браузере откройте страницу `http://<serveraddress>:<port>`, чтобы убедиться, что приложение локально работает на платформе Linux.

## <a name="configure-a-reverse-proxy-server"></a>Настройка обратного прокси-сервера

Обратный прокси-сервер — это стандартный вариант настройки для обслуживания динамических веб-приложений. Обратный прокси-сервер завершает HTTP-запрос и перенаправляет его в приложение ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

> [!NOTE]
> Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше. Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

### <a name="use-a-reverse-proxy-server"></a>Использование обратного прокси-сервера

Kestrel является отличным решением для обслуживания динамического содержимого из ASP.NET Core. При этом компоненты для работы с веб-службами не настолько функциональны, как серверы типа IIS, Apache или Nginx. Обратный прокси-сервер может облегчить такую работу, как обслуживание статического содержимого, кэширование запросов, сжатие запросов и завершение SSL с HTTP-сервера. Обратный прокси-сервер можно разместить на отдельном компьютере или развернуть параллельно с HTTP-сервером.

В контексте данного руководства используется отдельный экземпляр Nginx. Он выполняется на том же сервере, что и HTTP-сервер. Настройки можно выбирать в зависимости от требований.

Так как запросы перенаправляются обратным прокси-сервером, используйте [ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer), которое входит в пакет [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). Это ПО обновляет `Request.Scheme`, используя заголовок `X-Forwarded-Proto`, что обеспечивает правильную работу URI перенаправления и других политик безопасности.

Любой компонент, который зависит от схемы, например проверка подлинности, генерация ссылок, перенаправление и геолокация, должен находиться после вызова ПО промежуточного слоя перенаправления заголовков. Как правило, ПО промежуточного слоя перенаправления заголовков должно выполняться до остального ПО промежуточного слоя, за исключением ПО промежуточного слоя для диагностики и обработки ошибок. Такой порядок гарантирует, что ПО промежуточного слоя, полагающееся на сведения о перенаправленных заголовках, может использовать значения заголовков для обработки.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Вызовите метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме. В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Вызывайте метод [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) в `Startup.Configure`, прежде чем вызвать [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity), [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) или другое ПО промежуточного слоя для проверки подлинности по аналогичной схеме. В ПО промежуточного слоя настройте перенаправление заголовков `X-Forwarded-For` и `X-Forwarded-Proto`:

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

По умолчанию доверенными считаются только прокси-серверы, работающие на localhost (127.0.0.1, [:: 1]). Если запросы между Интернетом и веб-сервером обрабатывают другие прокси-серверы или сети организации, добавьте их в список <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> или <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> с помощью <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>. Следующий пример добавляет доверенный прокси-сервер с IP-адресом 10.0.0.100 в ПО промежуточного слоя для перенаправления заголовков `KnownProxies` в `Startup.ConfigureServices`:

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

Дополнительные сведения см. в разделе <xref:host-and-deploy/proxy-load-balancer>.

### <a name="install-nginx"></a>Установка Nginx

Установите Nginx с помощью команды `apt-get`. Программа установки создает сценарий инициализации *systemd*, который запускает Nginx как управляющую программу при запуске системы. Следуйте инструкциям по установке для Ubuntu, которые представлены в [описании официальных пакетов Nginx для Debian и Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages).

> [!NOTE]
> Если необходимы дополнительные модули Nginx, может потребоваться создание Nginx из источника.

Так как Nginx устанавливается впервые, запустите его напрямую, выполнив следующую команду.

```bash
sudo service nginx start
```

В браузере должна открыться стартовая страница Nginx по умолчанию. Целевая страница доступна по адресу `http://<server_IP_address>/index.nginx-debian.html`.

### <a name="configure-nginx"></a>Настройка Nginx

Чтобы настроить Nginx как обратный прокси-сервер для перенаправления запросов в наше приложение ASP.NET Core, измените файл */etc/nginx/sites-available/default*. Откройте этот файл в текстовом редакторе и замените его содержимое на следующий код.

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

Если отсутствуют совпадения для `server_name`, Nginx использует сервер по умолчанию. Если сервер по умолчанию не определен, первый сервер в файле конфигурации является сервером по умолчанию. Рекомендуется добавить в качестве сервера по умолчанию определенный сервер, который возвращает код состояния 444 в файле конфигурации. Ниже приведен пример конфигурации сервера по умолчанию:

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

С представленным выше файлом конфигурации и сервером по умолчанию Nginx принимает трафик от любого источника через порт 80 с заголовком узла `example.com` или `*.example.com`. Запросы, не соответствующие этим узлам, не будут перенаправляться в Kestrel. Запросы, которые им соответствуют, Nginx перенаправляет в Kestrel по адресу `http://localhost:5000`. Для получения дополнительной информации см. статью [Как nginx обрабатывает запросы](https://nginx.org/docs/http/request_processing.html). Сведения о том, как изменить IP-адрес/порт Kestrel, см. в разделе [Kestrel: конфигурация конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!WARNING]
> Если не будет указана правильная [директива server_name](https://nginx.org/docs/http/server_names.html), приложение будет подвержено значительным уязвимостям. Привязки с подстановочными знаками на уровне дочерних доменов (например, `*.example.com`) не создают таких угроз безопасности, если вы полностью контролируете родительский домен (в отличие от варианта `*.com`, создающего уязвимость). Дополнительные сведения см. в документе [rfc7230, раздел 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).

Установив конфигурацию Nginx, выполните команду `sudo nginx -t`, чтобы проверить синтаксис файлов конфигурации. Если проверка файла конфигурации прошла успешно, заставьте Nginx принять изменения, выполнив команду `sudo nginx -s reload`.

Для непосредственного запуска приложений на сервере:

1. Перейдите в каталог приложения.
1. Запустите исполняемый файл приложения: `./<app_executable>`.

Если возникает ошибка прав доступа, измените права доступа:

```console
chmod u+x <app_executable>
```

Если приложение выполняется на сервере, но не отвечает по Интернету, проверьте брандмауэр сервера и убедитесь, что порт 80 открыт. При использовании виртуальной машины Ubuntu Azure добавьте правило группы безопасности сети (NSG), которое разрешает входящий трафик через порт 80. Не нужно включать правило исходящего трафика на порте 80, так как исходящий трафик предоставляется автоматически при включении правила для входящего трафика.

Когда закончите тестировать приложение, завершите его работу с помощью `Ctrl+C` в командной строке.

## <a name="monitoring-the-app"></a>Мониторинг приложения

Сервер настроен на перенаправление запросов к `http://<serveraddress>:80` в приложение ASP.NET Core, выполняемое в Kestrel по адресу `http://127.0.0.1:5000`. При этом Nginx не настроен для управления процессом Kestrel. Для запуска и мониторинга соответствующего веб-приложения можно использовать *systemd* и создать файл службы. *systemd* — это система инициализации, предоставляющая различные функции для запуска и остановки процессов, а также управления ими. 

### <a name="create-the-service-file"></a>Создание файла службы

Создайте файл определения службы.

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Далее представлен пример файла службы для нашего приложения.

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Если пользователь *www-data* в конфигурации не используется, необходимо сначала создать определенного в примере пользователя, а затем предоставить ему необходимые права владения для файлов.

Используйте `TimeoutStopSec`, чтобы настроить время ожидания до завершения работы приложения после начального сигнала прерывания. Если приложение не завершит работу в течение этого периода, оно прерывается сигналом SIGKILL. Укажите значение в секундах без единиц измерения (например, `150`), значение интервала (например, `2min 30s`) или значение `infinity`, которое отключает время ожидания. По умолчанию `TimeoutStopSec` принимает значение `DefaultTimeoutStopSec` в файле конфигурации диспетчера (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*). В большинстве дистрибутивов по умолчанию устанавливается время ожидания 90 секунд.

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

Linux имеет файловую систему, в которой учитывается регистр символов. Если для параметра ASPNETCORE_ENVIRONMENT установить значение Production, будет выполнен поиск файла конфигурации *appsettings.Production.json*, а не файла *appsettings.production.json*.

Некоторые значения (например, строки подключения SQL) необходимо экранировать, чтобы поставщики конфигурации могли читать переменные среды. Используйте следующую команду, чтобы создать правильно экранированное значение для файла конфигурации:

```console
systemd-escape "<value-to-escape>"
```

Сохраните файл и включите службу.

```bash
sudo systemctl enable kestrel-hellomvc.service
```

Запустите службу и убедитесь, что она работает.

```
sudo systemctl start kestrel-hellomvc.service
sudo systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Теперь, когда обратный прокси-сервер настроен и systemd управляет процессом Kestrel, веб-приложение можно считать полностью настроенным и вы можете обратиться к нему по адресу `http://localhost` из браузера на локальном компьютере. Оно также доступно для удаленных компьютеров, несмотря на наличие блокирующих трафик межсетевых экранов. Заголовок `Server` в ответе подтверждает, что приложение ASP.NET Core обслуживается Kestrel.

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Просмотр журналов

Так как веб-приложение, использующее Kestrel, управляется через `systemd`, все события и процессы регистрируются в централизованном журнале. При этом журнал содержит все записи обо всех службах и процессах, управляемых `systemd`. Чтобы просмотреть элементы, связанные с `kestrel-hellomvc.service`, используйте следующую команду.

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Кроме того, количество возвращаемых записей можно уменьшить, указав параметры времени, например `--since today`, `--until 1 hour ago` или их комбинацию.

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a>Защита данных

[Стек защиты данных в ASP.NET Core](xref:security/data-protection/index) используется определенным [ПО промежуточного слоя](xref:fundamentals/middleware/index) ASP.NET Core, включая промежуточное ПО для проверки подлинности (например, промежуточное ПО файлов cookie) и средствами защиты от подделки межсайтовых запросов. Даже если API-интерфейсы защиты данных не вызываются из пользовательского кода, необходимо настроить защиту данных для создания постоянного [хранилища криптографических ключей](xref:security/data-protection/implementation/key-management). Если защита данных не настроена, ключи хранятся в памяти и удаляются при перезапуске приложения.

Если набор ключей хранится в памяти, при перезапуске приложения происходит следующее:

* Все токены аутентификации, использующие файлы cookie, становятся недействительными.
* При выполнении следующего запроса пользователю требуется выполнить вход снова.
* Все данные, защищенные с помощью набора ключей, больше не могут быть расшифрованы. Это могут быть [токены CSRF](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) и [файлы cookie временных данных ASP.NET Core MVC](xref:fundamentals/app-state#tempdata).

Сведения о настройке защиты данных для хранения и шифрования набора ключей см. в приведенных ниже статьях.

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="securing-the-app"></a>Защита приложения

### <a name="enable-apparmor"></a>Включение AppArmor

Начиная с версии 2.6 ядро Linux включает платформу модулей безопасности Linux (LSM). LSM поддерживают различные реализации модулей безопасности. [AppArmor](https://wiki.ubuntu.com/AppArmor) — это LSM, который реализует систему обязательного контроля доступа, позволяющую ограничивать программу определенным набором ресурсов. Убедитесь, что AppArmor включен и правильно настроен.

### <a name="configuring-the-firewall"></a>Настройка межсетевого экрана

Закройте все внешние порты, которые не используются. Незамысловатый межсетевой экран (ufw) позволяет взаимодействовать с `iptables`, предоставляя интерфейс командной строки для настройки межсетевого экрана.

> [!WARNING]
> Неправильно настроенный брандмауэр предотвратит доступ ко всей системе. Если задать неправильный порт SSH, то при использовании SSH для подключения к системе произойдет ее блокировка. Порт по умолчанию — 22. Дополнительные сведения см. в [вводной статье по ufw](https://help.ubuntu.com/community/UFW) и в [этом руководстве](http://manpages.ubuntu.com/manpages/bionic/man8/ufw.8.html).

Установите `ufw` и настройте его для разрешения прохождения трафика через все необходимые порты.

```bash
sudo apt-get install ufw

sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

sudo ufw enable
```

### <a name="securing-nginx"></a>Защита Nginx

#### <a name="change-the-nginx-response-name"></a>Изменение имени ответа Nginx

Внесите изменения в файл *src/http/ngx_http_header_filter_module.c*:

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-options"></a>Настройка параметров

Настройте дополнительные обязательные модули на сервере. Для дополнительной защиты приложения можно использовать межсетевой экран для веб-приложений, например [ModSecurity](https://www.modsecurity.org/).

#### <a name="configure-ssl"></a>Настройка SSL

* Настройте сервер для прослушивания трафика HTTPS через порт `443`, указав действительный сертификат, выпущенный доверенным центром сертификации (ЦС).

* Обеспечьте дополнительную защиту, применив некоторые методы, показанные в представленном ниже файле */etc/nginx/nginx.conf*. Это может быть выбор более строгого шифра и перенаправление всего HTTP-трафика в HTTPS.

* При добавлении заголовка `HTTP Strict-Transport-Security` (HSTS) все последующие запросы клиента будут проходить только по протоколу HTTPS.

* Если в дальнейшем планируется отключить SSL, не добавляйте заголовок Strict-Transport-Security или выберите соответствующий `max-age`.

Добавьте файл конфигурации */etc/nginx/proxy.conf*:

[!code-nginx[](linux-nginx/proxy.conf)]

Измените файл конфигурации */etc/nginx/proxy.conf*. В этом примере показаны разделы `http` и `server` одного и того же файла конфигурации.

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Защита Nginx от кликджекинга
Кликджекинг — это способ обмана, предназначенный для перехвата кликов пострадавшего пользователя. Кликджекинг обманом заставляет пользователей (посетителей) щелкать зараженные сайты. Для защиты сайта используйте X-FRAME-OPTIONS.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Необходимые компоненты для .NET Core в Linux](/dotnet/core/linux-prerequisites)
* [Nginx: двоичные выпуски. Официальные пакеты Debian и Ubuntu](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/#official-debian-ubuntu-packages)
* [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer)
* [NGINX. Использование перенаправленного заголовка](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/)
