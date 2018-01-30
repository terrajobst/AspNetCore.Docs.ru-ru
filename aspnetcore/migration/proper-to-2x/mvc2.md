---
title: "Миграция с ASP.NET на ASP.NET Core 2.0"
author: isaac2004
description: "Получите рекомендации для переноса существующих приложений ASP.NET MVC или веб-API ASP.NET 2.0 Core."
manager: wpickett
ms.author: scaddie
ms.date: 08/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc2
ms.openlocfilehash: 65717c1605c7f55bfd836110072772fe3dcdeb76
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Миграция с ASP.NET на ASP.NET Core 2.0

Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)

Данная статья служит руководством по миграции приложений ASP.NET на ASP.NET Core 2.0.

## <a name="prerequisites"></a>Предварительные требования

* [Пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии.

## <a name="target-frameworks"></a>Требуемые версии .NET Framework
Проекты ASP.NET Core 2.0 предлагают разработчикам гибкость работы с .NET Core и .NET Framework. Определить наиболее подходящую платформу поможет статья [Выбор между .NET Core и .NET Framework для серверных приложений](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

При разработке для .NET Framework проекты должны ссылаться на отдельные пакеты NuGet.

Работа с .NET Core позволяет избавиться от многочисленных явных ссылок на пакеты, благодаря [метапакету](xref:fundamentals/metapackage) ASP.NET Core 2.0. Установите метапакет `Microsoft.AspNetCore.All` в свой проект.

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

При использовании метапакета никакие указанные по ссылкам пакеты с приложением не развертываются. Все эти ресурсы входят в хранилище среды выполнения .NET Core и предварительно компилируются для повышения производительности. Дополнительные сведения см. в статье [Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2](xref:fundamentals/metapackage).

## <a name="project-structure-differences"></a>Различия в структуре пакетов
В ASP.NET Core упрощен формат файла *.csproj*. Вот некоторые основные изменения.
- Чтобы файлы считались частью проекта, включать их явно теперь не требуется. Это уменьшает вероятность конфликтов слияния XML при работе в больших командах.
- GUID-ссылки на другие проекты не используются, что повышает удобочитаемость файла.
- Файл можно редактировать, не выгружая его в Visual Studio.

    ![Параметр изменения файла CSPROJ в контекстном меню в Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Замена файла Global.asax
В ASP.NET Core появился новый механизм для начальной загрузки приложения. Точкой входа для приложений ASP.NET стал файл *Global.asax*. Такие задачи, как конфигурация маршрута, а также регистрации фильтров и областей, теперь выполняются в файле *Global.asax*.

[!code-csharp[Main](samples/globalasax-sample.cs)]

При этом подходе приложение сопоставляется с сервером, на котором оно развертывается, таким образом, чтобы это не мешало реализации. Чтобы отделить приложение от сервера, был введен [OWIN](http://owin.org/), который обеспечивает более точный способ совместного использования нескольких платформ. OWIN предоставляет конвейер для добавления только необходимых модулей. Среда внешнего размещения принимает функцию [Startup](xref:fundamentals/startup) для настройки служб и конвейера обработки запросов приложения. `Startup` регистрирует набор ПО промежуточного слоя в приложении. Для каждого запроса приложение вызывает каждый из компонентов ПО промежуточного слоя с головным указателем связанного списка в существующий набор обработчиков. Каждый компонент ПО промежуточного слоя может добавлять в конвейер обработки запросов один обработчик или несколько. Это достигается путем возвращения ссылки на обработчик, который представляет собой новый заголовок списка. Каждый обработчик отвечает за запоминание и вызов следующего обработчика в списке. При работе с ASP.NET Core точкой входа в приложения становится `Startup`, и зависимость от файла *Global.asax* исчезает. Используя OWIN с .NET Framework, применяйте для конвейера код следующего вида:

[!code-csharp[Main](samples/webapi-owin.cs)]

Он определяет ваши маршруты по умолчанию и изначально предусматривает XmlSerialization по Json. При необходимости добавьте другое ПО промежуточного слоя для этого конвейера (загрузка служб, параметры конфигурации, статические файлы и т. д.).

ASP.NET Core использует аналогичный подход, но не требует OWIN для обработки запроса. Вместо этого применяется метод *Program.cs* `Main` (как в консольных приложениях) и `Startup` загружается через него.

[!code-csharp[Main](samples/program.cs)]

`Startup` должен включать метод `Configure`. В `Configure` добавьте в конвейер необходимое ПО промежуточного слоя. В следующем примере (на основе шаблона веб-сайта по умолчанию) используются несколько методов расширения для настройки конвейера с поддержкой следующих компонентов.

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Страницы ошибок
* Статические файлы
* ASP.NET Core MVC
* идентификации

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Сервер и приложение разделены, что позволит вам в будущем легко перейти на другую платформу.

**Примечание**. Более подробный справочник по запуску ASP.NET Core и ПО промежуточного слоя см. в статье [Запуск в ASP.NET Core](xref:fundamentals/startup).

## <a name="storing-configurations"></a>Сохранение конфигураций
ASP.NET поддерживает параметры хранения. Эти параметры используются, например, для поддержки среды, в которой развертываются приложения. Как правило, все пользовательские пары ключей и значений хранятся в разделе `<appSettings>` файла *Web.config*:

[!code-xml[Main](samples/webconfig-sample.xml)]

Приложения считывают эти параметры с помощью коллекции `ConfigurationManager.AppSettings` в пространстве имен `System.Configuration`:

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core может сохранять данные конфигурации для приложения из любого файла и загружать их в процессе начальной загрузки ПО промежуточного слоя. По умолчанию в шаблонах проектов используется файл *appsettings.json*:

[!code-json[Main](samples/appsettings-sample.json)]

Загрузка этого файла в экземпляр `IConfiguration` в вашем приложении выполняется в файле *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

Для получения этих параметров приложение считывает данные из `Configuration`:

[!code-csharp[Main](samples/read-appsettings.cs)]

Существуют расширения, повышающие надежность этого подхода, например, [внедрение зависимостей](xref:fundamentals/dependency-injection) позволяет загружать эти значения в службу. Метод внедрения зависимостей предоставляет строго типизированный набор объектов конфигурации.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Примечание**. Более подробное руководство по конфигурации ASP.NET Core см. в статье [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Внедрение зависимостей в машинном коде
При сборке больших, масштабируемых приложений важно обеспечить слабые взаимозависимости между компонентами и службами. [Внедрение зависимостей](xref:fundamentals/dependency-injection) — популярный способ решения этой задачи и собственный компонент ASP.NET Core.

В приложениях ASP.NET разработчики используют для внедрения зависимостей стороннюю библиотеку. Одна из таких библиотек, [Unity](https://github.com/unitycontainer/unity), входит в шаблоны и рекомендации Майкрософт. 

Пример внедрения зависимостей с использованием библиотеки Unity — это реализация `IDependencyResolver` как оболочки для `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Создайте экземпляр `UnityContainer`, зарегистрируйте свою службу и установите средство разрешения зависимостей `HttpConfiguration` в новый экземпляр `UnityResolver` для вашего контейнера:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Там, где необходимо, вставьте `IProductRepository`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Так как внедрение зависимостей является частью ASP.NET Core, вы можете добавить свою службу в метод `ConfigureServices` файла *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

Репозиторий, как и в Unity, можно внедрять где угодно.

**Примечание**. Подробное руководство по внедрению зависимостей в ASP.NET Core см. в статье [Внедрение зависимостей в ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container).

## <a name="serving-static-files"></a>Обработку статических файлов
Важной частью разработки веб-приложений является возможность обслуживания статических ресурсов на стороне клиента. Наиболее распространенные примеры статических файлов — это HTML, каскадные таблицы стилей, Javascript и изображения. Эти файлы необходимо сохранять в место публикации приложения (или CDN) и указывать по ссылкам, чтобы они могли загружаться по запросу. В ASP.NET Core ситуация изменилась.

В ASP.NET статические файлы хранятся в разных папках со ссылками в представлениях.

В ASP.NET Core, если не заданы другие настройки, статические файлы хранятся на корневом веб-узле (*&lt;содержимое корневой папки&gt;/wwwroot*). Файлы загружаются в конвейер запросов путем вызова метода расширения `UseStaticFiles` из `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

**Примечание**. Для работы с .NET Framework установите пакет NuGet `Microsoft.AspNetCore.StaticFiles`.

Например, ресурс изображения в папке *wwwroot/images* доступен для браузера в расположении `http://<app>/images/<imageFileName>`.

**Примечание**. Более подробное руководство по обработке статических файлов в ASP.NET Core см. в статье [Введение в работу со статическими файлами в ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Перенос библиотек в .NET Core](/dotnet/core/porting/libraries)
