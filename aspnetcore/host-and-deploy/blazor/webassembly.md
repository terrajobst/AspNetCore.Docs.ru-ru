---
title: Размещение и развертывание Blazor WebAssembly с помощью ASP.NET Core
author: guardrex
description: Узнайте, как размещать и развертывать приложение Blazor с помощью ASP.NET Core, сетей доставки содержимого (CDN), файловых серверов и GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 943dbb772d9a7bcb337012c126828d1ab4eb545c
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816064"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a>Размещение и развертывание Blazor WebAssembly с помощью ASP.NET Core

Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

С [моделью размещения Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly):

* Приложение Blazor, его зависимости и среда выполнения .NET скачиваются в браузере.
* Приложение выполняется непосредственно в потоке пользовательского интерфейса браузера.

Поддерживаются следующие стратегии развертывания:

* Приложение Blazor обслуживается приложением ASP.NET Core. Эта стратегия описана в разделе [Размещенное развертывание с использованием ASP.NET Core](#hosted-deployment-with-aspnet-core).
* Приложение Blazor помещается на статическом веб-сервере или службе, где .NET не используется, чтобы обслуживать приложение Blazor. Эта стратегия рассматривается в разделе [Автономное развертывание](#standalone-deployment), где приводятся сведения о размещении приложения Blazor WebAssembly в качестве дочернего приложения IIS.

## <a name="rewrite-urls-for-correct-routing"></a>Переопределение URL-адресов для маршрутизации

Маршрутизация запросов к компонентам страниц в приложении Blazor WebAssembly не настолько проста, как маршрутизация запросов к приложению, размещенному на сервере Blazor. Рассмотрим приложение Blazor WebAssembly с двумя компонентами:

* *Main.razor* — загружается в корне приложения и содержит ссылку на компонент `About` (`href="About"`).
* *About.razor* — компонент `About`.

При запросе документа приложения по умолчанию в адресной строке браузера (например, `https://www.contoso.com/`):

1. Браузер отправляет запрос.
1. Возвращается страница по умолчанию; обычно это *index.html*.
1. Страница *index.html* обеспечивает начальную загрузку приложения.
1. Загружается маршрутизатор Blazor, и компонент `Main` Razor преобразовывается для просмотра.

На странице Main возможность выбора ссылки на компонент `About` доступна на клиентском компьютере, так как маршрутизатор Blazor не разрешает браузеру сделать запрос в Интернете к `www.contoso.com` для `About` и сам обрабатывает отображенный компонент `About`. Все запросы внутренних конечных точек *в пределах приложения Blazor WebAssembly* будут работать так же: Запросы не активируют браузерные запросы к ресурсам, размещенным на сервере в Интернете. Маршрутизатор обрабатывает запросы внутренним образом.

Если запрос `www.contoso.com/About` сделан с помощью адресной строки браузера, он завершится ошибкой. Такой ресурс не существует на интернет-узле приложения, поэтому возвращается ответ *404 — Not Found* (не найдено).

Поскольку браузеры выполняют запросы к интернет-узлам для страниц на стороне клиента, веб-серверам и службам размещения необходимо переопределять все запросы к ресурсам, физически находящимся не на сервере, в случае страницы *index.html*. Когда *index.html* возвращается, маршрутизатор Blazor приложения берет на себя управление и отвечает необходимым ресурсом.

При развертывании на сервере IIS можно использовать модуль переопределения URL-адресов с опубликованным файлом приложения *web.config*. Дополнительные сведения см. в описании [IIS](#iis).

## <a name="hosted-deployment-with-aspnet-core"></a>Размещенное развертывание с использованием ASP.NET Core

*Размещенное развертывание*  обслуживает приложение Blazor WebAssembly для браузеров из [приложения ASP.NET Core](xref:index), выполняющегося на веб-сервере.

Приложение Blazor входит в состав приложения ASP.NET Core в публикуемых выходных данных; таким образом, два приложения развертываются вместе. Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным. Для размещенного развертывания Visual Studio включает шаблон проекта **Приложение Blazor WebAssembly**  (шаблон `blazorwasm` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)) с выбранным параметром **Размещенный**.

Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.

Подробнее о развертывании в службе приложений Azure см. в <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Автономное развертывание

*Автономное развертывание* обрабатывает приложение Blazor WebAssembly как набор статических файлов, которые будут запрашиваться непосредственно клиентами. Любой статический файловый сервер способен обслуживать приложение Blazor.

Изолированные ресурсы развертывания публикуются в папке *bin/Release/{целевая_платформа}publish/{имя_сборки}/dist*.

### <a name="iis"></a>IIS

IIS можно использовать как полнофункциональный статический файловый сервер для приложений Blazor. Чтобы настроить IIS для размещения Blazor, см. раздел [Построение статического веб-сайта на IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Опубликованные ресурсы создаются в папке */bin/Release/{целевая_платформа}/publish*. Разместить содержимое папки *publish* на веб-сервере или в службе размещения.

#### <a name="webconfig"></a>web.config.

При публикации проекта Blazor создается файл *web.config* со следующей конфигурацией IIS:

* Типы MIME заданы для следующих расширений файлов:
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* Сжатие HTTP включено для следующих типов MIME:
  * `application/octet-stream`
  * `application/wasm`
* Задаются правила модуля переопределения URL-адресов:
  * Обслуживание вложенного каталога, в котором находятся статические ресурсы приложения ( *{имя_сборки}/dist/{запрошенный_путь}* ).
  * Создание резервного маршрута одностраничного приложения, чтобы запросы, не относящиеся к файлам ресурсов, перенаправлялись к документу приложения по умолчанию в папке статических ресурсов ( *{имя_сборки}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>Установка модуля переопределения URL-адресов

[Модуль переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite) нужен, чтобы переопределять URL-адреса. Модуль не устанавливается по умолчанию и недоступен для установки как компонент службы роли веб-сервера (IIS). Модуль необходимо скачать с веб-сайта IIS. Для его установки используйте установщик веб-платформы.

1. Локально перейдите на [страницу скачивания модуля переопределения URL-адресов](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). В англоязычной версии выберите **WebPI** для скачивания установщика веб-платформы. Для других языков выберите подходящую архитектуру сервера (x86 или x64) для скачивания установщика.
1. Скопируйте установщик на сервер. Запустите установщик. Нажмите кнопку **Установить** и примите условия лицензионного соглашения. Перезагрузка сервера после завершения установки не требуется.

#### <a name="configure-the-website"></a>Настройка веб-сайта

Установите для веб сайта **физический путь** к папке приложения. Папка содержит:

* Файл *web.config*, который используется службами IIS для настройки веб-сайта, включая необходимые правила перенаправления и типы содержимого файлов.
* Папку статических ресурсов приложения.

#### <a name="host-as-an-iis-sub-app"></a>Размещение в качестве дочернего приложения IIS

Если автономное приложение размещено как дочернее приложение IIS, выполните одно из следующих действий:

* Отключите наследуемый обработчик модуля ASP.NET Core.

  Удалите обработчик из опубликованного файла приложения Blazor *web.config*, добавив в файл раздел `<handlers>`:

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* Отключите наследование раздела `<system.webServer>` от корневого (родительского) приложения, используя `<location>` со значением `false` для `inheritInChildApplications`:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

Удаление обработчика или отключение наследования выполняется вместе с [настройкой базового пути приложения](xref:host-and-deploy/blazor/index#app-base-path). Задайте базовый путь приложения в файле *index.html* приложения как псевдоним IIS, используемый при настройке дочернего приложения в IIS.

#### <a name="troubleshooting"></a>Устранение неполадок

Если получено сообщение *500 — Internal Server Error* (внутренняя ошибка сервера), а диспетчер служб IIS возвращает ошибки при попытке получить доступ к конфигурации веб сайта, убедитесь, что установлен модуль переопределения URL-адресов. Если модуль не установлен, IIS не может проанализировать файл *web.config*. Это предотвращает загрузку конфигурации веб сайта диспетчером служб IIS и передачу статических файлов Blazor c веб-сайта.

Дополнительные сведения об устранении неполадок развертывания в службах IIS см. в разделе <xref:test/troubleshoot-azure-iis>.

### <a name="azure-storage"></a>Хранилище Azure

Размещение статических файлов [службы хранилища Azure](/azure/storage/) предусматривает размещение бессерверного приложения Blazor. Поддерживаются пользовательские доменные имена, сеть доставки содержимого Azure (CDN) и HTTPS.

Если служба BLOB-объектов настроена для размещения статических веб-сайтов в учетной записи хранения:

* Задайте для параметра **Имя документа индекса** значение `index.html`.
* Задайте для параметра **Путь к документу с ошибкой** значение `index.html`. Компоненты Razor и другие конечные точки, не являющиеся файлами, не имеют физических путей в статическом содержимом, хранящемся в службе больших двоичных объектов. При получении запроса для одного из этих ресурсов, который должен обрабатываться маршрутизатором Blazor, ошибка *404 — Not Found* (не найдено), создаваемая службой BLOB-объектов, перенаправляет запрос, используя **путь к документу с ошибкой**. При этом возвращается большой двоичный объект *index.html*, и маршрутизатор Blazor загружает и обрабатывает путь.

См. подробнее о [размещении статических веб-сайтов в службе хранилища Azure](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

Следующий файл *nginx.conf* упрощен для демонстрации того, как настроить Nginx для отправки файла *Index.html*, когда не удается найти соответствующий файл на диске.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Дополнительные сведения о конфигурации веб-сервера Nginx в рабочей среде см. в разделе [Создание файлов конфигурации NGINX Plus и NGINX](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).

### <a name="nginx-in-docker"></a>Nginx в Docker

Чтобы разместить Blazor в Docker с помощью Nginx, настройте в Dockerfile образ с Nginx на основе Alpine. Измените Dockerfile, скопировав файл *nginx.config* в контейнер.

Добавьте одну строку в Dockerfile, как показано в следующем примере:

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

Чтобы развернуть приложение Blazor WebAssembly в CentOS 7 или более поздней версии, выполните следующие действия:

1. Создайте файл конфигурации Apache. В следующем примере показан упрощенный файл конфигурации (*blazorapp.config*):

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType aplication/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. Поместите файл конфигурации Apache в каталог `/etc/httpd/conf.d/`. Это каталог конфигурации Apache по умолчанию в CentOS 7.

1. Поместите файлы приложения в каталог `/var/www/blazorapp` (расположение, указанное в параметре `DocumentRoot` в файле конфигурации).

1. Перезапустите службу Apache.

Дополнительные сведения см. в разделах [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) и [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).

### <a name="github-pages"></a>GitHub Pages

Чтобы обеспечить переопределение URL-адресов, добавьте файл *404.html* со сценарием, который производит перенаправление на страницу *index.html*. Пример реализации, предоставленный сообществом, см. в разделе [Одностраничные приложения для страниц GitHub](https://spa-github-pages.rafrex.com/) ([rafrex/spa-github-pages на сайте GitHub](https://github.com/rafrex/spa-github-pages#readme)). Пример с использованием подхода сообщества можно увидеть в разделе [blazor-demo/blazor-demo.github.io на сайте GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([активный сайт](https://blazor-demo.github.io/)).

При использовании сайта проекта вместо сайта организации следует добавить или обновить тег `<base>` в файле *index.html*. Задайте для имени репозитория GitHub значение атрибута `href` с косой чертой в конце (например, `my-repository/`).

## <a name="host-configuration-values"></a>Значения конфигурации узла

[Приложения Blazor WebAssembly](xref:blazor/hosting-models#blazor-webassembly) могут принимать следующие значения конфигурации узла в качестве аргументов командной строки во время выполнения в среде разработки.

### <a name="content-root"></a>Корневой каталог содержимого

Аргумент `--contentroot` задает абсолютный путь к каталогу, где находятся файлы содержимого приложения ([корневой каталог содержимого](xref:fundamentals/index#content-root)). В следующих примерах `/content-root-path` — это путь к корневому каталогу содержимого приложения.

* Передайте этот аргумент при локальном запуске приложения в командной строке. Перейдите в каталог приложения и выполните следующую команду:

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**. Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**. Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Базовый путь

Аргумент `--pathbase` задает базовый путь приложения для локального выполнения с некорневым относительным путем URL (в `<base>` тег `href` задан с путем, отличным от `/` для промежуточной и рабочей сред). В следующих примерах `/relative-URL-path` — это базовый путь приложения. Дополнительные сведения см. в описании [базового пути приложения](xref:host-and-deploy/blazor/index#app-base-path).

> [!IMPORTANT]
> В отличие от пути, заданного через `href` в теге `<base>`, не ставьте косую черту (`/`) при передаче значения аргумента `--pathbase`. Если основной путь приложения задан в теге `<base>` как `<base href="/CoolApp/">` (включая косую черту в конце), передайте значение аргумента командной строки как `--pathbase=/CoolApp` (без косой черты в конце).

* Передайте этот аргумент при локальном запуске приложения в командной строке. Перейдите в каталог приложения и выполните следующую команду:

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**. Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**. Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL-адреса

Аргумент `--urls` задает IP-адреса или адреса узлов с портами и протоколами, по которым ожидаются запросы.

* Передайте этот аргумент при локальном запуске приложения в командной строке. Перейдите в каталог приложения и выполните следующую команду:

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* Добавьте запись в файл приложения *launchSettings.json* в профиле **IIS Express**. Этот параметр используется при запуске приложения с помощью отладчика Visual Studio и из командной строки вместе с `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* В Visual Studio укажите аргумент в меню **Свойства** > **Отладка** > **Аргументы приложения**. Задание аргумента на странице свойств Visual Studio добавляет его в файл *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>Настройка компоновщика

Blazor выполняет компоновку для промежуточного языка (IL) в каждой сборке, чтобы удалить ненужный код из выходных сборок. Связыванием сборки можно управлять в процессе сборки. Дополнительные сведения можно найти по адресу: <xref:host-and-deploy/blazor/configure-linker>.
