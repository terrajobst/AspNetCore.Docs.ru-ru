---
title: "Использование с ASP.NET Core модули IIS"
author: guardrex
description: "Справочник по документ, описывающий активных и неактивных модули IIS для приложений ASP.NET Core."
keywords: "ASP.NET Core, iis, модуль, обратного прокси-сервера"
ms.author: riande
manager: wpickett
ms.date: 03/08/2017
ms.topic: article
ms.assetid: 492b3a7e-04c5-461b-b96a-38ecee5c64bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/iis-modules
ms.openlocfilehash: 353cd4c18cb2708f2dece5ba2b5271f452379d52
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="using-iis-modules-with-aspnet-core"></a>Использование с ASP.NET Core модули IIS

По [Latham Люк](https://github.com/GuardRex)

Приложения ASP.NET Core размещены в IIS в конфигурации обратного прокси-сервера. Некоторые собственные модули IIS и всех модулей IIS управляемых недоступны для обработки запросов для приложений ASP.NET Core. Во многих случаях ASP.NET Core является альтернативой функции собственные и управляемые модули IIS.

## <a name="native-modules"></a>Модули в машинном коде
Модуль | Активный .NET core | Параметр ASP.NET Core
--- | :---: | ---
**Анонимная проверка подлинности**<br>`AnonymousAuthenticationModule` | Да | 
**Обычная проверка подлинности**<br>`BasicAuthenticationModule` | Да | 
**Проверка подлинности с сопоставлением сертификации клиента**<br>`CertificateMappingAuthenticationModule` | Да | 
**CGI**<br>`CgiModule` | Нет | 
**Проверка конфигурации**<br>`ConfigurationValidationModule` | Да | 
**Ошибки HTTP**<br>`CustomErrorModule` | Нет | [По промежуточного слоя страницы кода состояния](xref:fundamentals/error-handling#configuring-status-code-pages)
**Настраиваемое протоколирование**<br>`CustomLoggingModule` | Да | 
**Документ по умолчанию**<br>`DefaultDocumentModule` | Нет | [По умолчанию файлы по промежуточного слоя](xref:fundamentals/static-files#serving-a-default-document)
**Дайджест-проверка подлинности**<br>`DigestAuthenticationModule` | Да | 
**Просмотр каталогов**<br>`DirectoryListingModule` | Нет | [По промежуточного слоя просмотра каталогов](xref:fundamentals/static-files#enabling-directory-browsing)
**Динамическое сжатие**<br>`DynamicCompressionModule` | Да | [По промежуточного слоя сжатия ответов](xref:performance/response-compression)
**Трассировка**<br>`FailedRequestsTracingModule` | Да | [Ведение журнала ASP.NET Core](xref:fundamentals/logging#the-tracesource-provider)
**Кэширование файлов**<br>`FileCacheModule` | Нет | [Кэширование ответов по промежуточного слоя](xref:performance/caching/middleware)
**Кэширование HTTP**<br>`HttpCacheModule` | Нет | [Кэширование ответов по промежуточного слоя](xref:performance/caching/middleware)
**Ведение журнала HTTP**<br>`HttpLoggingModule` | Да | [Ведение журнала ASP.NET Core](xref:fundamentals/logging)<br>Реализации: [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
**Перенаправление HTTP**<br>`HttpRedirectionModule` | Да | [По промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting)
**Проверка подлинности с сопоставлением сертификата клиента IIS**<br>`IISCertificateMappingAuthenticationModule` | Да | 
**Ограничения IP-адресов и доменов**<br>`IpRestrictionModule` | Да | 
**Фильтры ISAPI**<br>`IsapiFilterModule` | Да | [По промежуточного слоя](xref:fundamentals/middleware)
**ISAPI**<br>`IsapiModule` | Да | [По промежуточного слоя](xref:fundamentals/middleware)
**Поддержка протоколов**<br>`ProtocolSupportModule` | Да | 
**Фильтрация запросов**<br>`RequestFilteringModule` | Да | [По промежуточного слоя перезаписи URL-адрес`IRule`](xref:fundamentals/url-rewriting#irule-based-rule)
**Монитор запросов**<br>`RequestMonitorModule` | Да | 
**Переписывание URL-адресов**<br>`RewriteModule` | Yes† | [По промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting)
**Включения на стороне сервера**<br>`ServerSideIncludeModule` | Нет | 
**Статическое сжатие**<br>`StaticCompressionModule` | Нет | [По промежуточного слоя сжатия ответов](xref:performance/response-compression)
**Статическое содержимое**<br>`StaticFileModule` | Нет | [По промежуточного слоя статических файлов](xref:fundamentals/static-files)
**Кэширование маркеров**<br>`TokenCacheModule` | Да | 
**Кэширование URI**<br>`UriCacheModule` | Да | 
**Авторизация URL-адреса**<br>`UrlAuthorizationModule` | Да | [Удостоверение ASP.NET Core](xref:security/authentication/identity)
**Проверка подлинности Windows**<br>`WindowsAuthenticationModule` | Да | 

†The модуль переопределения URL-адрес `isFile` и `isDirectory` не работают с приложениями ASP.NET Core из-за изменений в [структуру каталогов](xref:hosting/directory-structure).

## <a name="managed-modules"></a>Управляемые модули
Модуль | Активный .NET core | Параметр ASP.NET Core
--- | :---: | ---
AnonymousIdentification | Нет | 
DefaultAuthentication | Нет | 
FileAuthorization | Нет | 
FormsAuthentication | Нет | [Файл cookie проверки подлинности по промежуточного слоя](xref:security/authentication/cookie)
OutputCache | Нет | [Кэширование ответов по промежуточного слоя](xref:performance/caching/middleware)
Профиль | Нет | 
RoleManager | Нет | 
ScriptModule 4.0 | Нет | 
Сеанс | Нет | [Сеанс по промежуточного слоя](xref:fundamentals/app-state)
UrlAuthorization | Нет | 
UrlMappingsModule | Нет | [По промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting)
UrlRoutingModule 4.0 | Нет | [Удостоверение ASP.NET Core](xref:security/authentication/identity)
WindowsAuthentication | Нет | 

## <a name="iis-manager-application-changes"></a>Изменения приложения диспетчера служб IIS
При использовании диспетчера служб IIS для настройки параметров вы меняете непосредственно *web.config* файл приложения. Если развертывание приложения и включают *web.config*, любые изменения, внесенные с помощью диспетчера IIS будут перезаписаны с развернутой *файл web.config*. Поэтому при внесении изменений на сервер *web.config* файл, скопировать обновленные *web.config* файл в локальном проекте немедленно.

## <a name="disabling-iis-modules"></a>Отключение модулей IIS
Если у вас есть модуль IIS настроен на уровне сервера, который вы хотите отключить для приложения, это можно сделать с дополнением к вашей *web.config* файла. Модуль следует оставить на месте и отключите его с помощью параметра конфигурации (при наличии) либо удалить модуль из приложения.

### <a name="module-deactivation"></a>Отключение модуля
Многие модули предоставляют параметр конфигурации, позволяющих сделать без их удаления из приложения. Это простой и быстрый способ деактивировать модуля. Например, если вы хотите отключить модуль переопределения URL-адреса IIS, используйте `<httpRedirect>` элемента, как показано ниже. Дополнительные сведения об отключении модулей с параметрами конфигурации по ссылкам в *дочерние элементы* раздел [IIS `<system.webServer>` ](https://docs.microsoft.com/iis/configuration/system.webServer/).

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

### <a name="module-removal"></a>Удаление модуля
Если вы решили удалить модуль с параметром *web.config*, необходимо разблокировать модуль и разблокировать `<modules>` раздел *web.config* первой. Шаги, описанные ниже:

1. Снятие блокировки модуля на уровне сервера. Нажмите кнопку на сервере IIS с помощью диспетчера IIS **подключений** боковой панели. Откройте **модули** в **IIS** области. Щелкните для модуля в списке. В **действия** боковой панели справа щелкните **Unlock**. Разблокировать столько модулей, которые планируется удалить с *web.config* позже.

2. Развернуть приложение без `<modules>` статьи *web.config*. Если развернуть приложение с *web.config* содержащий `<modules>` раздела без разблокирован раздел сначала с помощью диспетчера IIS, Configuration Manager будет вызывать исключение при попытке разблокировать раздел. Таким образом, развертывания приложения без `<modules>` раздела.

3. Разблокировать `<modules>` раздел *web.config*. В **подключений** боковой панели щелкните веб-сайт в **узлы**. В **управления** откройте **редактор конфигурации**. Позволяет выбрать элементы управления навигацией `system.webServer/modules` раздела. В **действия** боковой панели справа щелкните, чтобы **Unlock** разделе.

4. На этом этапе можно добавить `<modules>` раздел вашей *web.config* файл с `<remove>` элемент, чтобы удалить модуль из приложения. Можно добавить несколько `<remove>` элементы, чтобы удалить несколько модулей. Помните, что при внесении *web.config* изменения на сервере, чтобы сделать их непосредственно в проекте локально. Удаление модуля таким образом не влияет на использование модуля с другими приложениями на сервере.

  ```xml
  <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
  </configuration>
  ```

Для установки служб IIS с модулями по умолчанию установлен, можно использовать следующие `<module>` раздел, чтобы удалить модули по умолчанию.

```xml
<modules>
  <remove name="CustomErrorModule" />
  <remove name="DefaultDocumentModule" />
  <remove name="DirectoryListingModule" />
  <remove name="HttpCacheModule" />
  <remove name="HttpLoggingModule" />
  <remove name="ProtocolSupportModule" />
  <remove name="RequestFilteringModule" />
  <remove name="StaticCompressionModule" /> 
  <remove name="StaticFileModule" /> 
</modules>
```

Можно также удалить модуль IIS с *Appcmd.exe*. Укажите `MODULE_NAME` и `APPLICATION_NAME` в приведенной ниже команде:

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Вот как можно удалить `DynamicCompressionModule` с веб-сайта по умолчанию:

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimal-module-configuration"></a>Модуль минимальной конфигурации
Только модули, необходимые для запуска приложения ASP.NET Core: модуль анонимной проверки подлинности и модуль ASP.NET Core.

![Откройте диспетчер служб IIS с модулями с минимальной настройкой модулей показано](iis-modules/_static/modules.png)

## <a name="resources"></a>Ресурсы
* [Публикация в IIS](xref:publishing/iis)
* [Общие сведения о модули IIS](https://docs.microsoft.com/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Настройка IIS 7.0 ролей и модулей](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS`<system.webServer>`](https://docs.microsoft.com/iis/configuration/system.webServer/)
