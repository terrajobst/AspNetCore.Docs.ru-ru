---
title: Компиляция и предварительная компиляция представлений Razor в ASP.NET Core
author: rick-anderson
description: Узнайте, как включить компиляцию и предварительную компиляцию представлений MVC Razor в приложениях ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a>Компиляция файла Razor (.cshtml) в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Представления Razor компилируются во время выполнения при вызове представления. В ASP.NET Core 2.1.0 или более поздней версии представления компилируются во время сборки и публикации с помощью [пакета SDK для Razor](/aspnetcore/mvc/razor-pages/sdk). В ASP.NET Core 1.1 и ASP.NET Core 2.0 представления можно при необходимости компилировать во время публикации и развертывать с приложением &mdash; с помощью средств предварительной компиляции. 



Особенности предварительной компиляции:

* Предварительная компиляция представлений уменьшает размер публикуемого пакета и сокращает время запуска.
* После предварительной компиляции представлений редактировать файлы Razor невозможно. Отредактированные представления не попадут в опубликованный пакет. 

Развертывание предварительно скомпилированных представлений осуществляется следующим образом.

# <a name="aspnet-core-21tabaspnetcore21"></a>[ASP.NET Core 2.1](#tab/aspnetcore21/)
Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакетов SDK для Razor. Редактирование файлов Razor после их обновления поддерживается во время сборки. По умолчанию только скомпилированный файл *Views.dll*, а не CSHTML-файлы, развертываются вместе с приложением. 
    
> [!IMPORTANT]
> Пакет SDK для Razor — это единственное эффективное средство в случае, когда в файле проекта не заданы свойства для предварительной компиляции. Например, параметр `MvcRazorCompileOnPublish` в файле *CSPROJ* отключает пакет SDK для Razor.

# <a name="aspnet-core-20tabaspnetcore20"></a>[ASP.NET Core 2.0](#tab/aspnetcore20/)

Если проект нацелен на платформу .NET Framework, включите ссылку на пакет в [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Если проект нацелен на .NET Core, никаких изменений не требуется.

Шаблоны проектов ASP.NET Core 2.x неявно задают свойству `MvcRazorCompileOnPublish` значение `true` по умолчанию. Это означает, что этот узел можно безопасно удалить из файла *CSPROJ*.
    
> [!IMPORTANT]
> Предварительная компиляция представлений Razor сейчас недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0. 

Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish). Например, выполните следующую команду в корневом элементе проекта:

```console
dotnet publish -c Release
```

При успешной компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные представления Razor. Например, следующий снимок экрана показывает содержимое *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Задайте свойству `MvcRazorCompileOnPublish` значение `true` и включите ссылку на пакет в `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Это показано в следующем примере *CSPROJ*:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

