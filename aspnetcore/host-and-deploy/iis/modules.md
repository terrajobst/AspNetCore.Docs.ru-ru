---
title: Модули IIS с ASP.NET Core
author: guardrex
description: Обнаружение активных и неактивных модули IIS для приложения ASP.NET Core и управлении модули IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: e88526d997618658f58488adb37ae1e519ea3f59
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Модули IIS с ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Приложения ASP.NET Core размещаются в службах IIS в конфигурации обратного прокси-сервера. Некоторые собственные модули IIS и всех модулей IIS управляемых недоступны для обработки запросов для приложений ASP.NET Core. Во многих случаях ASP.NET Core является альтернативой функции собственные и управляемые модули IIS.

## <a name="native-modules"></a>Модули в машинном коде

В этой таблице перечислены собственные модули IIS, предназначенные для работы на запросы обратного прокси-сервера для приложений ASP.NET Core.

| Module | Режим работы с приложениями ASP.NET Core | Параметр ASP.NET Core |
| ------ | :-------------------------------: | ------------------- |
| **Анонимная проверка подлинности**<br>`AnonymousAuthenticationModule` | Да | |
| **Обычная проверка подлинности**<br>`BasicAuthenticationModule` | Да | |
| **Проверка подлинности с сопоставлением сертификации клиента**<br>`CertificateMappingAuthenticationModule` | Да | |
| **CGI**<br>`CgiModule` | Нет | |
| **Проверка конфигурации**<br>`ConfigurationValidationModule` | Да | |
| **Ошибки HTTP**<br>`CustomErrorModule` | Нет | [По промежуточного слоя страницы кода состояния](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Настраиваемое протоколирование**<br>`CustomLoggingModule` | Да | |
| **Документ по умолчанию**<br>`DefaultDocumentModule` | Нет | [По умолчанию файлы по промежуточного слоя](xref:fundamentals/static-files#serve-a-default-document) |
| **Дайджест-проверка подлинности**<br>`DigestAuthenticationModule` | Да | |
| **Просмотр каталогов**<br>`DirectoryListingModule` | Нет | [По промежуточного слоя просмотра каталогов](xref:fundamentals/static-files#enable-directory-browsing) |
| **Динамическое сжатие**<br>`DynamicCompressionModule` | Да | [ПО промежуточного слоя для сжатия ответов](xref:performance/response-compression) |
| **Трассировка**<br>`FailedRequestsTracingModule` | Да | [Ведение журнала ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider) |
| **Кэширование файлов**<br>`FileCacheModule` | Нет | [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware) |
| **Кэширование HTTP**<br>`HttpCacheModule` | Нет | [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware) |
| **Ведение журнала HTTP**<br>`HttpLoggingModule` | Да | [Ведение журнала ASP.NET Core](xref:fundamentals/logging/index)<br>Реализации: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Перенаправление HTTP**<br>`HttpRedirectionModule` | Да | [ПО промежуточного слоя для переопределения URL-адресов](xref:fundamentals/url-rewriting) |
| **Проверка подлинности с сопоставлением сертификата клиента IIS**<br>`IISCertificateMappingAuthenticationModule` | Да | |
| **Ограничения IP-адресов и доменов**<br>`IpRestrictionModule` | Да | |
| **Фильтры ISAPI**<br>`IsapiFilterModule` | Да | [ПО промежуточного слоя](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Да | [ПО промежуточного слоя](xref:fundamentals/middleware/index) |
| **Поддержка протоколов**<br>`ProtocolSupportModule` | Да | |
| **Фильтрация запросов**<br>`RequestFilteringModule` | Да | [По промежуточного слоя перезаписи URL-адрес `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Монитор запросов**<br>`RequestMonitorModule` | Да | |
| **Переписывание URL-адресов**<br>`RewriteModule` | Да&#8224; | [ПО промежуточного слоя для переопределения URL-адресов](xref:fundamentals/url-rewriting) |
| **Включения на стороне сервера**<br>`ServerSideIncludeModule` | Нет | |
| **Статическое сжатие**<br>`StaticCompressionModule` | Нет | [ПО промежуточного слоя для сжатия ответов](xref:performance/response-compression) |
| **Статическое содержимое**<br>`StaticFileModule` | Нет | [По промежуточного слоя статических файлов](xref:fundamentals/static-files) |
| **Кэширование маркеров**<br>`TokenCacheModule` | Да | |
| **Кэширование URI**<br>`UriCacheModule` | Да | |
| **Авторизация URL-адреса**<br>`UrlAuthorizationModule` | Да | [Удостоверение ASP.NET Core](xref:security/authentication/identity) |
| **Аутентификация Windows**<br>`WindowsAuthenticationModule` | Да | |

&#8224;Модуль переопределения URL-в `isFile` и `isDirectory` соответствует типам не работают с приложениями ASP.NET Core из-за изменений в [структуру каталогов](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Управляемые модули

Управляемые модули являются *не* функционален размещенного приложения ASP.NET Core, если заданная версия среды CLR .NET пула приложений **без управляемого кода**. ASP.NET Core предлагает варианты по промежуточного слоя в нескольких случаях.

| Module                  | Параметр ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Файл cookie проверки подлинности по промежуточного слоя](xref:security/authentication/cookie) |
| OutputCache             | [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware) |
| Профиль                 | |
| RoleManager             | |
| ScriptModule 4.0        | |
| Сеанс                 | [Сеанс по промежуточного слоя](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [ПО промежуточного слоя для переопределения URL-адресов](xref:fundamentals/url-rewriting) |
| UrlRoutingModule 4.0    | [Удостоверение ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Изменения приложения диспетчера служб IIS

При использовании диспетчера служб IIS для настройки параметров, *web.config* приложения был изменен. Если развертывание приложения, включая *web.config*, будут перезаписаны все изменения, внесенные с помощью диспетчера служб IIS с развернутой *web.config* файла. При внесении изменений на сервер *web.config* файл, скопировать обновленные *web.config* файла на сервере, чтобы немедленно локального проекта.

## <a name="disabling-iis-modules"></a>Отключение модулей IIS

Если модуль IIS настраивается на уровне сервера, который должен быть отключен для приложения, добавление в приложение *web.config* файла можно отключить модуль. Модуль следует оставить на месте и отключите его с помощью параметра конфигурации (при наличии) либо удалить модуль из приложения.

### <a name="module-deactivation"></a>Отключение модуля

Многие модули предоставляют параметр конфигурации, их можно отключить без удаления модуля из приложения. Это простой и быстрый способ деактивировать модуля. Например, можно отключить перенаправление HTTP-модуля с  **\<httpRedirect >** элемент в *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Дополнительные сведения об отключении модулей с параметрами конфигурации по ссылкам в *дочерние элементы* раздел [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Удаление модуля

Если включения защиты, чтобы удалить модуль с параметром *web.config*, модуль и разблокировке  **\<модули >** раздел *web.config* первого:

1. Снятие блокировки модуля на уровне сервера. В диспетчере IIS выберите сервер IIS **подключений** боковой панели. Откройте **модули** в **IIS** области. Выберите модуль в списке. В **действия** боковой панели справа выберите **Unlock**. Разблокировать столько модулей, которые планируется удалить из *web.config* позже.

2. Развернуть приложение без  **\<модули >** статьи *web.config*. Если приложение развертывается с *web.config* содержащий  **\<модули >** раздела без разблокирован раздел сначала с помощью диспетчера IIS, Configuration Manager возникло исключение При попытке разблокировать раздел. Таким образом, развертывание приложения без  **\<модули >** раздела.

3. Разблокировать  **\<модули >** раздел *web.config*. В **подключений** боковой панели выберите веб-сайт в **узлы**. В **управления** откройте **редактор конфигурации**. Позволяет выбрать элементы управления навигацией `system.webServer/modules` раздела. В **действия** боковой панели справа, выберите, чтобы **Unlock** разделе.

4. На этом этапе  **\<модули >** можно добавить раздел *web.config* файл с  **\<удалить >** элемента, удаляемого из модуля приложение. Несколько  **\<удалить >** могут быть добавлены элементы, чтобы удалить несколько модулей. Если *web.config* изменений на сервере, немедленно внесения изменений в проект *web.config* файл на компьютере. Удаление модуля таким образом не влияет на использование модуля с другими приложениями на сервере.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Также можно удалить модуль IIS с *Appcmd.exe*. Укажите `MODULE_NAME` и `APPLICATION_NAME` в команде:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Например, удалить `DynamicCompressionModule` с веб-сайта по умолчанию:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Минимальной настройкой модулей

Только модули, необходимые для запуска приложения ASP.NET Core: модуль анонимной проверки подлинности и модуль ASP.NET Core.

![Откройте диспетчер служб IIS с модулями с минимальной настройкой модулей показано](modules/_static/modules.png)

Модуль кэширования URI (`UriCacheModule`) позволяет IIS для веб-сайт конфигурации кэша на уровне URL-адрес. Без этого модуля IIS необходимо ознакомиться и синтаксический анализ конфигурации при каждом запросе, даже в том случае, если же URL-адрес запрашивается несколько раз. Синтаксический анализ конфигурации каждый запрос приводит к значительному снижению производительности. *Несмотря на то, что модуль кэширования URI не является обязательным для размещенного приложения ASP.NET Core для запуска, рекомендуется включить модуль кэширования URI для всех развертываний ASP.NET Core.*

Модуль кэширования HTTP (`HttpCacheModule`) реализует кэш вывода IIS, а также логику для кэширования элементы в кэше HTTP.sys. Без этого модуля содержимое больше не кэшируется в режиме ядра и профилей кэша учитываются. Удаление модуля кэширования HTTP обычно оказывает неблагоприятное воздействие на производительность и использование ресурсов. *Несмотря на то, что модуль кэширования HTTP не является обязательным для размещенного приложения ASP.NET Core для запуска, рекомендуется включить кэширование HTTP-модуля для всех развертываний ASP.NET Core.*

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Размещение в Windows с помощью IIS](xref:host-and-deploy/iis/index)
* [Введение в архитектуры IIS: модули в IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Общие сведения о модули IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Настройка IIS 7.0 ролей и модулей](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
