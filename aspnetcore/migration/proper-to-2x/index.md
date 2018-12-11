---
title: Миграция с ASP.NET на ASP.NET Core
author: isaac2004
description: Здесь вы найдете указания по миграции существующих приложений MVC ASP.NET или веб-API на ASP.NET Core.web
ms.author: scaddie
ms.date: 12/10/2018
uid: migration/proper-to-2x/index
ms.openlocfilehash: 6808fefb890dcdec6abdd0604ab61dfd2573d910
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216798"
---
# <a name="migrate-from-aspnet-to-aspnet-core"></a>Миграция с ASP.NET на ASP.NET Core

Автор [Айзек Левин](https://isaaclevin.com) (Isaac Levin)

Данная статья служит руководством по миграции приложений ASP.NET на ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

[!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

## <a name="target-frameworks"></a>Требуемые версии .NET Framework

Проекты ASP.NET Core предлагают разработчикам гибкость работы с .NET Core и .NET Framework. Определить наиболее подходящую платформу поможет статья [Выбор между .NET Core и .NET Framework для серверных приложений](/dotnet/standard/choosing-core-framework-server).

При разработке для .NET Framework проекты должны ссылаться на отдельные пакеты NuGet.

Работа с .NET Core позволяет избавиться от многочисленных явных ссылок на пакеты благодаря [метапакету](xref:fundamentals/metapackage) ASP.NET Core. Установите метапакет `Microsoft.AspNetCore.All` в свой проект.

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
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

[!code-csharp[](samples/globalasax-sample.cs)]

При этом подходе приложение сопоставляется с сервером, на котором оно развертывается, таким образом, чтобы это не мешало реализации. Чтобы отделить приложение от сервера, был введен [OWIN](http://owin.org/), который обеспечивает более точный способ совместного использования нескольких платформ. OWIN предоставляет конвейер для добавления только необходимых модулей. Среда внешнего размещения принимает функцию [Startup](xref:fundamentals/startup) для настройки служб и конвейера обработки запросов приложения. `Startup` регистрирует набор ПО промежуточного слоя в приложении. Для каждого запроса приложение вызывает каждый из компонентов ПО промежуточного слоя по заглавному указателю связанного списка на существующий набор обработчиков. Каждый компонент ПО промежуточного слоя может добавлять в конвейер обработки запросов один обработчик или несколько. Это достигается путем возвращения ссылки на обработчик, который представляет собой новый заголовок списка. Каждый обработчик отвечает за запоминание и вызов следующего обработчика в списке. При работе с ASP.NET Core точкой входа в приложения становится `Startup`, и зависимость от файла *Global.asax* исчезает. Используя OWIN с .NET Framework, применяйте для конвейера код следующего вида:

[!code-csharp[](samples/webapi-owin.cs)]

Он определяет ваши маршруты по умолчанию и изначально предусматривает XmlSerialization по Json. При необходимости добавьте другое ПО промежуточного слоя для этого конвейера (загрузка служб, параметры конфигурации, статические файлы и т. д.).

ASP.NET Core использует аналогичный подход, но не требует OWIN для обработки запроса. Вместо этого применяется метод *Program.cs* `Main` (как в консольных приложениях) и `Startup` загружается через него.

[!code-csharp[](samples/program.cs)]

`Startup` должен включать метод `Configure`. В `Configure` добавьте в конвейер необходимое ПО промежуточного слоя. В следующем примере (на основе шаблона веб-сайта по умолчанию) используются несколько методов расширения для настройки конвейера с поддержкой следующих компонентов.

* [Привязывание к браузеру](xref:client-side/using-browserlink)
* Страницы ошибок
* Статические файлы
* ASP.NET Core MVC
* идентификации

[!code-csharp[](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

Сервер и приложение разделены, что позволит вам в будущем легко перейти на другую платформу.

> [!NOTE]
> Более подробный справочник по запуску ASP.NET Core и ПО промежуточного слоя см. в статье [Запуск в ASP.NET Core](xref:fundamentals/startup).

## <a name="store-configurations"></a>Конфигурации хранения

ASP.NET поддерживает параметры хранения. Эти параметры используются, например, для поддержки среды, в которой развертываются приложения. Как правило, все пользовательские пары ключей и значений хранятся в разделе `<appSettings>` файла *Web.config*:

[!code-xml[](samples/webconfig-sample.xml)]

Приложения считывают эти параметры с помощью коллекции `ConfigurationManager.AppSettings` в пространстве имен `System.Configuration`:

[!code-csharp[](samples/read-webconfig.cs)]

ASP.NET Core может сохранять данные конфигурации для приложения из любого файла и загружать их в процессе начальной загрузки ПО промежуточного слоя. По умолчанию в шаблонах проектов используется файл *appsettings.json*:

[!code-json[](samples/appsettings-sample.json)]

Загрузка этого файла в экземпляр `IConfiguration` в вашем приложении выполняется в файле *Startup.cs*:

[!code-csharp[](samples/startup-builder.cs)]

Для получения этих параметров приложение считывает данные из `Configuration`:

[!code-csharp[](samples/read-appsettings.cs)]

Существуют расширения, повышающие надежность этого подхода, например, [внедрение зависимостей](xref:fundamentals/dependency-injection) позволяет загружать эти значения в службу. Метод внедрения зависимостей предоставляет строго типизированный набор объектов конфигурации.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

> [!NOTE]
> Более подробное руководство по конфигурации ASP.NET Core см. в статье [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Собственные функции внедрения зависимостей

При сборке больших, масштабируемых приложений важно обеспечить слабые взаимозависимости между компонентами и службами. [Внедрение зависимостей](xref:fundamentals/dependency-injection) — популярный способ решения этой задачи и собственный компонент ASP.NET Core.

В приложениях ASP.NET разработчики используют для внедрения зависимостей стороннюю библиотеку. Одна из таких библиотек, [Unity](https://github.com/unitycontainer/unity), входит в шаблоны и рекомендации Майкрософт.

Пример внедрения зависимостей с использованием библиотеки Unity — это реализация `IDependencyResolver` как оболочки для `UnityContainer`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Создайте экземпляр `UnityContainer`, зарегистрируйте свою службу и установите средство разрешения зависимостей `HttpConfiguration` в новый экземпляр `UnityResolver` для вашего контейнера:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Там, где необходимо, вставьте `IProductRepository`:

[!code-csharp[](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Так как внедрение зависимостей является частью ASP.NET Core, вы можете добавить свою службу в метод `ConfigureServices` файла *Startup.cs*:

[!code-csharp[](samples/configure-services.cs)]

Репозиторий, как и в Unity, можно внедрять где угодно.

> [!NOTE]
> Дополнительные сведения о внедрении зависимостей см. [здесь](xref:fundamentals/dependency-injection).

## <a name="serve-static-files"></a>Обслуживание статических файлов

Важной частью разработки веб-приложений является возможность обслуживания статических ресурсов на стороне клиента. Наиболее распространенные примеры статических файлов — это HTML, каскадные таблицы стилей, Javascript и изображения. Эти файлы необходимо сохранять в место публикации приложения (или CDN) и указывать по ссылкам, чтобы они могли загружаться по запросу. В ASP.NET Core ситуация изменилась.

В ASP.NET статические файлы хранятся в разных папках со ссылками в представлениях.

В ASP.NET Core, если не заданы другие настройки, статические файлы хранятся на корневом веб-узле (*&lt;содержимое корневой папки&gt;/wwwroot*). Файлы загружаются в конвейер запросов путем вызова метода расширения `UseStaticFiles` из `Startup.Configure`:

[!code-csharp[](../../fundamentals/static-files/samples/1x/StartupStaticFiles.cs?highlight=3&name=snippet_ConfigureMethod)]

> [!NOTE]
> Для работы с .NET Framework установите пакет NuGet `Microsoft.AspNetCore.StaticFiles`.

Например, ресурс изображения в папке *wwwroot/images* доступен для браузера в расположении `http://<app>/images/<imageFileName>`.

> [!NOTE]
> Более подробное руководство по обработке статических файлов в ASP.NET Core см. в статье [Статические файлы](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Дополнительные ресурсы

- [Перенос библиотек в .NET Core](/dotnet/core/porting/libraries)
