---
title: "Миграция с ASP.NET в ASP.NET 2.0 Core"
author: isaac2004
description: "Этот справочник документ содержит инструкции для переноса существующих приложений ASP.NET MVC или веб-API ASP.NET 2.0 Core."
keywords: "Миграция Core, MVC ASP.NET"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Миграция с ASP.NET в ASP.NET 2.0 Core

По [Isaac Levin](https://isaaclevin.com)

В этой статье служит руководство предназначено для переноса приложений ASP.NET в ASP.NET Core 2.0.

## <a name="prerequisites"></a>Предварительные требования

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) или более поздней версии.

## <a name="target-frameworks"></a>Требуемые версии .NET Framework
Проекты ASP.NET Core 2.0 предлагает разработчикам гибкость нацеливания .NET Core и .NET Framework. В разделе [Выбор между .NET Core и .NET Framework для сервера приложений](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) для определения наиболее подходящего какие требуемой версии .NET framework.

При разработке для .NET Framework, проекты должны ссылаться на отдельные пакеты NuGet.

Отбор информации о .NET Core позволяет устранить множество ссылок явную пакета, благодаря ASP.NET Core 2.0 [metapackage](xref:fundamentals/metapackage). Установка `Microsoft.AspNetCore.All` metapackage в проекте:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

При использовании metapackage нет пакетов, на которые ссылается metapackage развертываются с приложением. Хранилище среды выполнения .NET Core включает эти ресурсы, и они компилируются для повышения производительности. В разделе [metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x](xref:fundamentals/metapackage) для получения дополнительных сведений.

## <a name="project-structure-differences"></a>Различия в структуре проекта
*.Csproj* формат файла был упрощен в ASP.NET Core. Ниже перечислены некоторые важные изменения.
- Явное включение файлов не является обязательным для них считается частью проекта. Это уменьшает вероятность конфликтов слияния XML при работе на больших команд.
- Не существует на основе GUID ссылок на другие проекты, что повышает удобочитаемость файла.
- Файл можно изменять без выгрузки его в Visual Studio:

    ![Измените CSPROJ контекстного меню в Visual Studio 2017 г.](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Замена файла Global.asax
ASP.NET Core появился новый механизм для начальной загрузки приложения. Точка входа для приложений ASP.NET — *Global.asax* файла. Задачи, например настройка маршрутов и регистрации фильтров и область, обрабатываются в *Global.asax* файла.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Этот подход связывает приложением и сервером, на котором развертывается в результате которого мешает реализации. С целью отделения [OWIN](http://owin.org/) был введен для обеспечения более точный способ совместное использование нескольких платформ. OWIN предоставляет конвейера только те модули, которые требуется добавить. Среда размещения принимает [запуска](xref:fundamentals/startup) функции для настройки служб и конвейер обработки запросов приложения. `Startup`Регистрирует набор по промежуточного слоя в приложении. Для каждого запроса, приложение вызывает каждый из компонентов по промежуточного слоя с головного указателя к существующему набору обработчиков связанного списка. Каждый компонент по промежуточного слоя можно добавить один или несколько обработчиков для обработки конвейера запросов. Это достигается путем возвращения ссылку на обработчик, который нового заголовка списка. Каждый обработчик отвечает за запоминание и вызова обработчика далее в списке. С помощью ASP.NET Core является точкой входа приложения `Startup`, и больше не имеют зависимость от *Global.asax*. При использовании OWIN с .NET Framework, используйте следующим как конвейера:

[!code-csharp[Main](samples/webapi-owin.cs)]

Это настраивает ваш маршруты по умолчанию и по умолчанию используется значение XmlSerialization по Json. Добавьте другого по промежуточного слоя для данного конвейера (загрузка служб, параметры конфигурации, статические файлы, и т. д.).

ASP.NET Core подобный подход используется, но не зависит от OWIN для обработки операции. Вместо этого, выполняется с помощью *Program.cs* `Main` метода (аналогично консольных приложений) и `Startup` загружается через него.

[!code-csharp[Main](samples/program.cs)]

`Startup`необходимо включить `Configure` метод. В `Configure`, добавьте необходимые по промежуточного слоя в конвейере. В следующем примере (на основе шаблона веб-сайта по умолчанию) несколько методов расширения используются для настройки конвейера с поддержкой:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Страницы ошибок
* Статические файлы
* Основные ASP.NET MVC
* Удостоверение

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Основное приложение и приложение разделены, благодаря чему обеспечивается гибкость перехода на другой платформе, в будущем.

**Примечание:** более подробный справочник для запуска ASP.NET Core и по промежуточного слоя см [запуска в ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Сохранение конфигураций
ASP.NET поддерживает сохранения параметров. Эти параметры используются, например, для поддержки среды, к которому развернутых приложений. Было принято для хранения всех пользовательских пар ключ значение в `<appSettings>` раздел *Web.config* файла:

[!code-xml[Main](samples/webconfig-sample.xml)]

Приложения считывают эти параметры с помощью `ConfigurationManager.AppSettings` коллекции в `System.Configuration` пространство имен:

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core можно хранить данные конфигурации для приложения из любого файла и загрузить их как часть начальную загрузку по промежуточного слоя. Файл по умолчанию, используемые в шаблонах проектов *appsettings.json*:

[!code-json[Main](samples/appsettings-sample.json)]

Загрузить этот файл в экземпляр `IConfiguration` приложение выполняется в внутри *файла Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

Приложение считывает из `Configuration` для получения этих параметров:

[!code-csharp[Main](samples/read-appsettings.cs)]

В этот подход, чтобы облегчить процесс более надежными, например с помощью расширений [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) для загрузки службы с этими значениями. DI подход предоставляет набор строго типизированных объектов конфигурации.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Примечание:** более подробная справка по конфигурации ASP.NET Core, в разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration).

## <a name="native-dependency-injection"></a>Внедрение зависимостей в машинном коде
При построении больших масштабируемых приложений из важнейших целей — слабых связей между компонентами и службами. [Внедрение зависимостей](xref:fundamentals/dependency-injection) -это популярный способ для достижения этой цели, и это собственный компонент ASP.NET Core.

В приложениях ASP.NET разработчики используют стороннюю библиотеку для реализации внедрения зависимостей. Одна библиотека является [Unity](https://github.com/unitycontainer/unity), предоставляемого шаблонов и практик Майкрософт. 

Реализация пример настройки внедрение зависимостей в Unity `IDependencyResolver` -оболочки `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Создайте экземпляр вашего `UnityContainer`, зарегистрировать службу и установить средство разрешения зависимостей `HttpConfiguration` новому экземпляру `UnityResolver` для контейнера:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Вставить `IProductRepository` при необходимости:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Поскольку внедрения зависимостей является частью ASP.NET Core, можно добавить службу в `ConfigureServices` метод *файла Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

Репозитории могут быть добавлены в любом месте, как и в Unity.

**Примечание:** подробная справка по внедрение зависимостей в ASP.NET Core, в разделе [внедрение зависимостей в ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Обработку статических файлов
Важной частью разработки веб-приложений является возможность обслуживания статических, клиентского средства. Наиболее распространенными примерами статические файлы, HTML, CSS, Javascript и изображения. Эти файлы должны быть сохранены в каталог публикации приложения (или CDN) и ссылки, поэтому они могут быть загружены по запросу. Этот процесс был изменен в ASP.NET Core.

В ASP.NET статические файлы хранятся в разных каталогах и ссылки в представлениях.

В ASP.NET Core статические файлы хранятся в «root web» (*&lt;содержимое корневого&gt;/wwwroot*), если не заданы другие настройки. Файлы загружаются в конвейер запросов путем вызова `UseStaticFiles` метод расширения из `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

**Примечание:** Если нужна платформа .NET Framework, установите пакет NuGet `Microsoft.AspNetCore.StaticFiles`.

Например, ресурс изображения в *wwwroot/images* папка доступна в браузер в расположении например `http://<app>/images/<imageFileName>`.

**Примечание:** более подробный справочник для обработки статических файлов в ASP.NET Core см [введение в работу с статических файлов в ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Дополнительные ресурсы
* [Перенос библиотеки для .NET Core](https://docs.microsoft.com/dotnet/core/porting/libraries)
