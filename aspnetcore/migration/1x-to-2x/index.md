---
title: "Миграция с ASP.NET Core 1.x на 2.0"
author: scottaddie
description: "В этой статье описываются предварительные требования и стандартные этапы миграции проекта ASP.NET Core 1.x в ASP.NET Core 2.0."
keywords: "ASP.NET Core, миграция"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: db277ee6b079eab973a565983d6661bf95fce2e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>Миграция с ASP.NET Core 1.x на ASP.NET Core 2.0

Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)

В этой статье поэтапно рассматривается процесс обновления существующего проекта ASP.NET Core 1.x до версии ASP.NET Core 2.0. Миграция приложения в ASP.NET Core 2.0 позволяет воспользоваться [множеством новых функций и улучшений производительности](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0). 

Существующие приложения ASP.NET Core 1.x основаны на шаблонах проектов для определенной версии. По мере развития платформы ASP.NET Core совершенствуются шаблоны проектов и содержащийся в них начальный код. Помимо обновления платформы ASP.NET Core, необходимо также обновить код приложения.

<a name="prerequisites"></a>

## <a name="prerequisites"></a>Предварительные требования
См. статью [Начало работы с ASP.NET Core](xref:getting-started).

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>Обновление моникера целевой платформы (TFM)
Проекты, предназначенные для .NET Core, должны использовать [моникер целевой платформы](/dotnet/standard/frameworks#referring-to-frameworks) версии не ниже .NET Core 2.0. Найдите в файле *CSPROJ* узел `<TargetFramework>` и замените его содержимое на `netcoreapp2.0`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]

Проекты, предназначенные для .NET Framework, должны использовать моникер целевой платформы версии не ниже .NET Framework 4.6.1. Найдите в файле *CSPROJ* узел `<TargetFramework>` и замените его содержимое на `net461`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET Core 2.0 обеспечивает гораздо большую контактную зону по сравнению с .NET Core 1.x. Если вы используете .NET Framework только из-за отсутствия нужных API в .NET Core 1.x, скорее всего, .NET Core 2.0 удовлетворит ваши потребности.

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>Обновление версии пакета SDK для .NET Core в файле global.json
Если ваше решение использует файл [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) для указания целевой версии пакета SDK для .NET Core, измените значение свойства `version` так, чтобы использовалась версия 2.0, установленная на компьютере:

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>Обновление ссылок на пакеты
В файле *CSPROJ* в проекте версии 1.x перечислены все проекты NuGet, используемые проектом.

В проекте ASP.NET Core 2.0, предназначенном для .NET Core 2.0, коллекция пакетов в файле *CSPROJ* заменяется ссылкой на один [метапакет](xref:fundamentals/metapackage):

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]

В метапакет входят все компоненты ASP.NET Core 2.0 и Entity Framework Core 2.0.

В проектах ASP.NET Core 2.0, предназначенных для .NET Framework, по-прежнему должны использоваться ссылки на отдельные пакеты NuGet. Измените значение атрибута `Version` каждого узла `<PackageReference />` на 2.0.0.

Например, вот список узлов `<PackageReference />`, используемых в типичном проекте ASP.NET Core 2.0, предназначенном для .NET Framework:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>Обновление средств CLI для .NET Core
Измените значение атрибута * каждого узла * в файле `Version`CSPROJ`<DotNetCliToolReference />` на 2.0.0.

Например, вот список средств CLI, используемых в типичном проекте ASP.NET Core 2.0, предназначенном для .NET Core 2.0:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>Переименование свойства PackageTargetFallback
В проектах версии 1.x в файлах *CSPROJ* использовался узел `PackageTargetFallback` и переменная:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]

Переименуйте этот узел и переменную в `AssetTargetFallback`:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>Обновление метода Main в файле Program.cs
В проектах версии 1.x метод `Main` в файле *Program.cs* выглядел так:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

В проектах версии 2.0 метод `Main` в файле *Program.cs* упрощен:

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]

Настоятельно рекомендуется внедрить этот новый шаблон версии 2.0, так как он необходим для работы таких функций, как [миграция Entity Framework Core](xref:data/ef-mvc/migrations). Например, при выполнении команды `Update-Database` из консоли диспетчера пакетов или команды `dotnet ef database update` в командной строке (для проектов, преобразованных в ASP.NET Core 2.0) возникает следующая ошибка:

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>Проверка параметра компиляции представлений Razor
Сокращение времени запуска приложений и уменьшение размеров публикуемых пакетов крайне важны. По этой причине в ASP.NET Core 2.0 по умолчанию включена [компиляция представлений Razor](xref:mvc/views/view-compilation).

Присваивать свойству `MvcRazorCompileOnPublish` значение true больше не нужно. Если вы не собираетесь отключать компиляцию представлений, это свойство можно удалить из файла *CSPROJ*.

Если проект предназначен для .NET Framework, необходимо по-прежнему явно указывать ссылку на пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) в файле *CSPROJ*:

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>Использование функций подготовки Application Insights
Простота настройки инструментария для обеспечения производительности приложений имеет большое значение. Теперь вам доступны новые функции подготовки [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) в составе средств Visual Studio 2017.

При создании проектов ASP.NET Core 1.1 в Visual Studio 2017 служба Application Insights добавлялась по умолчанию. Если вы не используете пакет SDK для Application Insights напрямую вне файлов *Program.cs* и *Startup.cs*, выполните указанные ниже действия.

1. Удалите из файла *CSPROJ* следующий узел `<PackageReference />`:
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]

2. Удалите вызов метода расширения `UseApplicationInsights` из файла *Program.cs*:

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. Удалите вызов API на стороне клиента Application Insights из файла *_Layout.cshtml*. Этот вызов включает две строки кода:

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]

Если вы используете пакет SDK для Application Insights напрямую, продолжайте делать это. [Метапакет](xref:fundamentals/metapackage) версии 2.0 включает в себя последнюю версию Application Insights, поэтому при попытке сослаться на более старую версию возникает ошибка понижения уровня пакета.

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>Внедрение улучшений, связанных с проверкой подлинности и удостоверениями
В ASP.NET Core 2.0 реализована новая модель проверки подлинности и внесен ряд важных изменений в удостоверение ASP.NET Core. Если при создании проекта вы включили отдельные учетные записи пользователей либо вручную добавили проверку подлинности или удостоверение, см. статью [Миграция на другой метод проверки подлинности и другие удостоверения в ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).

## <a name="additional-resources"></a>Дополнительные ресурсы
- [Критические изменения в ASP.NET Core 2.0](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
