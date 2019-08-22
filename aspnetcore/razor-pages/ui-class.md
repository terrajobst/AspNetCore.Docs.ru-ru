---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: В этой статье описывается создание многократно используемых Razor пользовательского интерфейса, использование частичных представлений в библиотеку классов, в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 468d961c291810ca4dfbe615acd972cfd6e7572a
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886393"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Создание повторно используемого пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Представления Razor, страницы, контроллеры, модели страниц, [компоненты Razor](xref:blazor/class-libraries), [Компоненты представления](xref:mvc/views/view-components)и модели данных могут быть встроены в БИБЛИОТЕКУ классов Razor (РКЛ). RCL можно упаковать и использовать повторно. Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы. При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.

Эта функция требует [!INCLUDE[](~/includes/2.1-SDK.md)]

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Создание библиотеки классов с пользовательским интерфейсом Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Назовите библиотеку (например, "RazorClassLib") > **OK**. Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Библиотека классов Razor** > **OK**.

РКЛ имеет следующий файл проекта:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните из командной строки команду `dotnet new razorclasslib`. Пример:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new). Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.

---

Добавление файлов Razor в RCL.

Шаблоны ASP.NET Core считает содержимое RCL *областей* папки. Чтобы создать РКЛ, который предоставляет содержимое в, а не `~/Areas/Pages`в `~/Pages` , см. раздел [РКЛ Pages Layout](#afs) .

## <a name="referencing-rcl-content"></a>Ссылка на содержимое РКЛ

На RCL могут ссылаться:

* Пакет NuGet. См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Пошаговое руководство. Создание проекта РКЛ и использование из проекта Razor Pages

Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его. Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта. Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098).

### <a name="test-the-download-app"></a>Тестирование приложения загрузки

Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Откройте *SLN*-файл в Visual Studio. Запустите приложение.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В командной строке в каталоге *cli* создайте RCL и веб-приложение.

```console
dotnet build
```

Перейдите в каталог *WebApp1* и запустите приложение:

```console
dotnet run
```

---

Следуйте инструкциям в разделе [Тестирование WebApp1](#test)

## <a name="create-an-rcl"></a>Создание РКЛ

В этом разделе создается РКЛ. Файлы Razor будут добавлены в RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Создание проекта RCL:

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Присвойте приложению имя **RazorUIClassLib** > **ОК**.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Библиотека классов Razor** > **OK**.
* Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду в командной строке:

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Предыдущие команды:

* `RazorUIClassLib` Создает РКЛ.
* Создает страницу Razor _Message и добавляет ее в RCL. Параметр `-np` создает страницу без `PageModel`.
* Создает [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) файл и добавляет его к RCL.

*_ViewStart.cshtml* файл необходим для использования макет проекта Razor Pages (который добавляется в следующем разделе).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Добавьте в проект Razor файлов и папок

* Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`). Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*. Пример:

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Дополнительные сведения о *_ViewImports.cshtml*, см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)

* Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:

```console
dotnet build RazorUIClassLib
```

Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*. В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Создание веб-приложения Razor Pages:

* В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Назовите приложение **WebApp1**.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Веб-приложение** > **OK**.

* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.
* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.
* Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.
* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.
* В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.

Запустите приложение.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Создайте Razor Pages веб-приложение и файл решения, содержащий приложение Razor Pages и РКЛ:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Выполните сборку и запустите приложения:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Тестирование WebApp1

Убедитесь, что библиотека классов пользовательского интерфейса Razor используется:

* Перейдите по адресу `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Переопределение представлений, частичных представлений и страниц

При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении. Например, добавьте в РКЛ элемент " *APP1/Areas/мифеатуре/Pages/* ", а также "стр. cshtml".

В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.

Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Обновите разметку, чтобы указать новое расположение. Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Макет страниц RCL

Ссылка RCL содержимого, как если бы, если он является частью веб приложения *страниц* папки, создайте проект RCL с со следующей структурой файла:

* *RazorUIClassLib/страниц*
* *RazorUIClassLib/страниц/Shared*

Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов: *_Header.cshtml* и *_Footer.cshtml*. `<partial>` Удалось добавить теги *_Layout.cshtml* файла:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a>Создание РКЛ со статическими ресурсами

РКЛ может потребовать сопутствующих статических ресурсов, на которые может ссылаться приложение, использующее РКЛ. ASP.NET Core позволяет создавать Рклс, которые включают статические ресурсы, доступные для использования в приложении.

Чтобы включить сопутствующие активы в состав РКЛ, создайте папку *wwwroot* в библиотеке классов и включите в нее все необходимые файлы.

При упаковке РКЛ все сопутствующие ресурсы в папке *wwwroot* автоматически включаются в пакет.

### <a name="consume-content-from-a-referenced-rcl"></a>Использовать содержимое из РКЛ, на которое указывает ссылка

Файлы, содержащиеся в папке *wwwroot* РКЛ, предоставляются приложению, использующему префикс `_content/{LIBRARY NAME}/`. Например, Библиотека с именем *Razor. class. lib* приводит к получению пути к статическому содержимому `_content/Razor.Class.Lib/`в.

Используемое приложение ссылается на статические ресурсы `<script>` `<img>`, предоставляемые библиотекой `<style>`, с помощью,, и других HTML-тегов. В используемом приложении должна быть включена [Поддержка статических файлов](xref:fundamentals/static-files) в `Startup.Configure`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

При запуске приложения, использующего выход из сборки (`dotnet run`), статические веб-ресурсы по умолчанию включены в среде разработки. Чтобы обеспечить поддержку ресурсов в других средах при выполнении из выходных данных `UseStaticWebAssets` сборки, вызовите метод в построителе узлов в *Program.CS*:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

Вызов `UseStaticWebAssets` не требуется при запуске приложения из опубликованных выходных данных`dotnet publish`().

### <a name="multi-project-development-flow"></a>Последовательность разработки нескольких проектов

При запуске работающего приложения:

* Ресурсы в РКЛ остаются в исходных папках. Ресурсы не перемещаются в приложение, использующее использование.
* Любые изменения в папке *wwwroot* РКЛ отражаются в приложении после перестроения РКЛ и без перестроения приложения, использующего приложение.

При построении РКЛ создается манифест, описывающий статические расположения веб-ресурсов. Использующее приложение считывает манифест во время выполнения для использования ресурсов из проектов и пакетов, на которые имеются ссылки. Когда новый ресурс добавляется в РКЛ, РКЛ необходимо перестраивать, чтобы обновить его манифест, прежде чем использование приложения сможет получить доступ к новому ресурсу.

### <a name="publish"></a>Публикация

При публикации приложения сопутствующие ресурсы из всех упоминаемых проектов и пакетов копируются в папку *wwwroot* опубликованного приложения в разделе `_content/{LIBRARY NAME}/`.

::: moniker-end
