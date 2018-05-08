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
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Компиляция и предварительная компиляция представлений Razor в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Представления Razor компилируются во время выполнения при вызове представления. ASP.NET Core 1.1.0 и более поздние версии позволяют при необходимости компилировать представления Razor отдельно и развертывать их с приложением &mdash; (процесс, называемый "предварительной компиляцией"). Шаблоны проектов ASP.NET Core 2.x поддерживают предварительную компиляцию по умолчанию.

> [!IMPORTANT]
> Предварительная компиляция представлений Razor сейчас недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0. Эта функция станет доступна для автономных развертываний в версии 2.1. Дополнительные сведения: [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Сбой компиляции представления при перекрестной компиляции для Linux в Windows).

Особенности предварительной компиляции:

* Предварительная компиляция представлений уменьшает размер публикуемого пакета и сокращает время запуска.
* После предварительной компиляции представлений редактировать файлы Razor невозможно. Отредактированные представления не попадут в опубликованный пакет. 

Развертывание предварительно скомпилированных представлений осуществляется следующим образом.

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Если проект нацелен на платформу .NET Framework, включите ссылку на пакет в [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Если проект нацелен на .NET Core, никаких изменений не требуется.

Шаблоны проектов ASP.NET Core 2.x неявно задают свойству `MvcRazorCompileOnPublish` значение `true` по умолчанию. Это означает, что этот узел можно безопасно удалить из файла *CSPROJ*. Если вы предпочитаете работать явным образом, задание свойству `MvcRazorCompileOnPublish` значения `true` не принесет никакого вреда. Это показано в следующем примере *CSPROJ*:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Задайте свойству `MvcRazorCompileOnPublish` значение `true` и включите ссылку на пакет в `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Это показано в следующем примере *CSPROJ*:

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish). Например, выполните следующую команду в корневом элементе проекта:

```console
dotnet publish -c Release
```

При успешной компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные представления Razor. Например, следующий снимок экрана показывает содержимое *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)
