---
title: "Новые возможности ASP.NET Core 2.0"
author: rick-anderson
description: "Новые возможности ASP.NET Core 2.0"
keywords: "ASP.NET Core,заметки о выпуске,новые возможности"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: 08c9f457-9c24-40f9-a08b-47dc251e4cec
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-2.0
ms.openlocfilehash: 2efb1ef63c378d84461da4f2d2f890423a670dda
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="whats-new-in-aspnet-core-20"></a>Новые возможности ASP.NET Core 2.0

В этой статье описываются наиболее важные изменения в ASP.NET Core 2.0 со ссылками на соответствующую документацию.

## <a name="razor-pages"></a>Razor Pages

Razor Pages — это новая функция платформы MVC ASP.NET Core, которая делает создание кодов сценариев для страниц проще и эффективнее.

Дополнительные сведения см. в следующей вводной статье и учебнике.

* [Введение в Razor Pages](xref:mvc/razor-pages/index)
* [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start)

## <a name="aspnet-core-metapackage"></a>Метапакет ASP.NET Core

Новый метапакет ASP.NET Core включает все пакеты, выпущенные и поддерживаемые командами ASP.NET Core и Entity Framework Core, а также внутренние и сторонние зависимости. Вам больше не придется выбирать отдельный пакет компонентов ASP.NET Core. Все компоненты входят в пакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All). Шаблоны по умолчанию используют именно этот пакет.

Дополнительные сведения см. в статье [Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.0](xref:fundamentals/metapackage).

## <a name="runtime-store"></a>Хранилище среды выполнения

Приложения, использующие метапакет `Microsoft.AspNetCore.All`, автоматически получают все преимущества нового хранилища среды выполнения .NET Core. Хранилище содержит все ресурсы среды выполнения, необходимые для запуска приложений ASP.NET Core 2.0. При использовании метапакета `Microsoft.AspNetCore.All` приложение не развертывает никакие ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core, так как эти пакеты уже присутствуют в целевой системе. Кроме того, для сокращения времени запуска приложения ресурсы в хранилище среды выполнения подвергаются предварительной компиляции.

Дополнительные сведения см. в статье [Хранилище среды выполнения](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).

## <a name="net-standard-20"></a>.NET Standard 2.0

Пакеты ASP.NET 2.0 предназначены для .NET Standard 2.0. На эти пакеты могут ссылаться другие библиотеки .NET Standard 2.0; кроме того, они могут выполняться в реализациях .NET, совместимых с .NET Standard 2.0, включая .NET Core 2.0 и .NET Framework 4.6.1. 

Метапакет `Microsoft.AspNetCore.All` работает только с .NET Core 2.0, так как предназначен для использования с хранилищем среды выполнения .NET Core 2.0.

## <a name="configuration-update"></a>Изменения в конфигурации

В ASP.NET Core 2.0 экземпляр `IConfiguration` добавляется в контейнер служб по умолчанию. `IConfiguration` в контейнере служб упрощает для приложений задачу получения значений конфигурации из контейнера.

Сведения о состоянии плановой документации см. в статье о [проблемах GitHub](https://github.com/aspnet/Docs/issues/3387).

## <a name="logging-update"></a>Изменения в ведении журналов

В ASP.NET 2.0 Core ведение журнала по умолчанию включено в систему внедрения зависимостей. Добавление поставщиков и настройка фильтрации выполняются в файле *Program.cs*, а не в файле *Startup.cs*. А `ILoggerFactory` по умолчанию поддерживает такой способ фильтрации, который позволяет использовать один гибкий подход и для перекрестной фильтрации по поставщикам, и для фильтрации по отдельному поставщику.

Дополнительные сведения см. в статье [Введение в ведение журналов](xref:fundamentals/logging).

## <a name="authentication-update"></a>Изменения в проверке подлинности

Новая модель проверки подлинности облегчает настройку проверки подлинности для приложения с использованием внедрения зависимостей.

Доступные новые шаблоны для настройки проверки подлинности в веб-приложениях и веб-API с использованием [Azure AD B2C] (https://azure.microsoft.com/services/active-directory-b2c/).

Сведения о состоянии плановой документации см. в статье о [проблемах GitHub](https://github.com/aspnet/Docs/issues/3054).

## <a name="identity-update"></a>Изменения в удостоверениях

Мы упростили сборку защищенных веб-API с использованием удостоверений в ASP.NET Core 2.0. Теперь маркеры доступа для обращения к веб-API можно получить с помощью [библиотеки проверки подлинности Microsoft (MSAL)](https://www.nuget.org/packages/Microsoft.Identity.Client).

Дополнительные сведения об изменениях в проверке подлинности в версии 2.0 см. в следующих ресурсах.

* [Подтверждение учетной записи и восстановление пароля в ASP.NET Core](xref:security/authentication/accconfirm)
* [Включение создания QR-кодов для приложений проверки подлинности в ASP.NET Core](xref:security/authentication/identity-enable-qrcodes)
* [Миграция на другой метод проверки подлинности и другие удостоверения в ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="spa-templates"></a>Шаблоны SPA

Доступны шаблоны проектов одностраничных приложений (Single Page Application, SPA) Angular, Aurelia, Knockout.js, React.js и React.js с Redux. Шаблон Angular обновлен до Angular 4. Шаблоны Angular и React доступны по умолчанию. Сведения о получении других шаблонов см. в разделе [Создание проекта SPA](xref:client-side/spa-services#creating-a-new-project). Сведения о сборке SPA в ASP.NET Core см. в разделе [Создание одностраничных приложение с помощью JavaScriptServices](xref:client-side/spa-services).

## <a name="kestrel-improvements"></a>Усовершенствования Kestrel

Веб-сервер Kestrel получил новые функции, необходимые серверу с выходом в Интернет. Мы добавили несколько параметров конфигурации ограничений для сервера в новое свойство `Limits` класса `KestrelServerOptions`. Теперь можно добавлять следующие ограничения:

- максимальное число клиентских подключений;
- максимальный размер текста запроса;
- минимальная скорость передачи данных в тексте запроса.

Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel).

## <a name="weblistener-renamed-to-httpsys"></a>WebListener переименован в HTTP.sys

Пакеты `Microsoft.AspNetCore.Server.WebListener` и `Microsoft.Net.Http.Server` объединены в новый пакет `Microsoft.AspNetCore.Server.HttpSys`. Соответственно обновлены и пространства имен.

Дополнительные сведения см. в статье [Реализация веб-сервера HTTP.sys в ASP.NET Core](xref:fundamentals/servers/httpsys).

## <a name="enhanced-http-header-support"></a>Расширенная поддержка заголовков HTTP

Теперь при использовании MVC для передачи `FileStreamResult` или `FileContentResult` можно указывать дату `ETag` или `LastModified` для передаваемого содержимого. Для возвращаемого содержимого эти значения можно задать, используя следующий код:

```csharp
var data = Encoding.UTF8.GetBytes("This is a sample text from a binary array");
var entityTag = new EntityTagHeaderValue("\"MyCalculatedEtagValue\"");
return File(data, "text/plain", "downloadName.txt", lastModified: DateTime.UtcNow.AddSeconds(-5), entityTag: entityTag);
```

Файл, возвращаемый вашим посетителям, будет оформлен соответствующими заголовками HTTP для значений `ETag` и `LastModified`.

Если посетители вашего приложение запросят содержимое с заголовком типа "Запрос диапазона", ASP.NET распознает и обработает этот заголовок. Запрошенное содержимое может доставляться частично, в случае чего ASP.NET соответствующим образом пропустит и вернет только запрошенный набор байтов.  При этом прописывать в методах специальные обработчики для адаптации или реализации этой функции не требуется, все будет сделано автоматически.

## <a name="hosting-startup-and-application-insights"></a>Запуск внешнего размещения и Application Insights

Среды внешнего размещения теперь внедряют зависимости дополнительных пакетов и выполняют код во время запуска приложения; при этом приложению не нужно явно принимать зависимость или вызывать какие-либо методы. Эту функцию можно использовать для того, чтобы выделить в определенных средах какие-то уникальные для них компоненты без предварительной настройки самого приложения. 

В ASP.NET Core 2.0 эта функция используется для автоматического включения диагностики Application Insights при отладке в Visual Studio и (после включения) при запуске службы приложений Azure. В связи с этим шаблоны проектов больше не добавляют пакеты и код Application Insights по умолчанию.

Сведения о состоянии плановой документации см. в статье о [проблемах GitHub](https://github.com/aspnet/Docs/issues/3389).

## <a name="automatic-use-of-anti-forgery-tokens"></a>Автоматическое использование маркеров защиты от подделки

ASP.NET Core всегда помогает в создании HTML-кода для содержимого, но в новой версии сделан еще один шаг к предотвращению атак с подделкой межсайтовых запросов (XSRF). Теперь ASP.NET Core будет выдавать маркеры защиты от подделки по умолчанию и проверять их при отправке форм и выполнении страниц без дополнительной конфигурации.

Дополнительные сведения см. в статье [Предотвращение атак с подделкой межсайтовых запросов (XSRF/CSRF) в ASP.NET Core](xref:security/anti-request-forgery).

## <a name="automatic-precompilation"></a>Автоматическая предварительная компиляция

При публикации по умолчанию включается предварительная компиляция представления Razor, что сокращает размер выходных данных публикации и время запуска приложения.

## <a name="razor-support-for-c-71"></a>Поддержка Razor для C# 7.1

Для работы с новым компилятором Roslyn был обновлен и обработчик представлений Razor. Добавлена поддержка таких функций C# 7.1 как выражения по умолчанию, выводимые имена кортежей и сопоставление шаблонов с универсальными шаблонами. Чтобы использовать C# 7.1 в проекте, добавьте в файл проекта следующее свойство и перезагрузите решение.

```xml
<LangVersion>latest</LangVersion>
```

Сведения о состоянии компонентов C# 7.1 см. в статье о [репозитории Roslyn GitHub](https://github.com/dotnet/roslyn/blob/master/docs/Language%20Feature%20Status.md).

## <a name="other-documentation-updates-for-20"></a>Изменения в другой документации к версии 2.0

* [Создание профилей публикации для Visual Studio и MSBuild с целью развертывания приложений ASP.NET Core](xref:publishing/web-publishing-vs)
* [Управление ключами](xref:security/data-protection/implementation/key-management)
* [Настройка проверки подлинности Facebook](xref:security/authentication/facebook-logins)
* [Настройка проверки подлинности Twitter](xref:security/authentication/twitter-logins)
* [Настройка проверки подлинности Google](xref:security/authentication/google-logins)
* [Настройка проверки подлинности учетной записи Майкрософт](xref:security/authentication/microsoft-logins)
* [Настройка HTTPS для разработки в ASP.NET Core](xref:security/https)

## <a name="migration-guidance"></a>Руководство по миграции

Инструкции по миграции приложений с ASP.NET Core 1.x в ASP.NET 2.0 см. в следующих ресурсах.

* [Миграция с ASP.NET Core 1.x на ASP.NET Core 2.0](xref:migration/1x-to-2x/index)
* [Миграция на другой метод проверки подлинности и другие удостоверения в ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)

## <a name="additional-information"></a>Дополнительные сведения

Полный список изменений см. в статье [Заметки о выпуске ASP.NET Core 2.0](https://github.com/aspnet/Home/releases/tag/2.0.0).

Если вы хотите отслеживать ход работы и планы команды разработчиков ASP.NET Core, смотрите еженедельные выпуски [ASP.NET Community Standup](https://live.asp.net/).
