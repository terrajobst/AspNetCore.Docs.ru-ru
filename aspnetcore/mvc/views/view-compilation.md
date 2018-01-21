---
title: "Компиляция представления Razor и предварительной компиляции"
author: rick-anderson
description: "Ссылки документов о том, как включить компиляции представления MVC Razor и предварительной компиляции в приложениях ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Компиляция представления Razor и предварительной компиляции в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Представлений Razor компилируются во время выполнения при вызове представления. ASP.NET Core 1.1.0 и более поздней версии можно при необходимости компиляции представлений Razor и развертываются с приложением&mdash;этот процесс называется предварительной компиляции. Предварительная компиляция включает шаблоны проектов ASP.NET Core 2.x по умолчанию.

> [!IMPORTANT]
> Предварительная компиляция представления Razor в настоящее время недоступна, при выполнении [автономное развертывание (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0. Компонент будет доступен для SCD, когда освобождает 2.1. Дополнительные сведения см. в разделе [представление компиляция завершается с ошибкой при перекрестной компиляции для Linux в Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).

Замечания по предварительной компиляции.

* Предкомпиляция представлений приведет к более мелкие опубликованный пакет и уменьшение времени запуска.
* После предкомпиляция представлений не может редактировать файлы Razor. Редактируемого представления не будет присутствовать в опубликованный пакет. 

Развертывание предварительно скомпилированного представления:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если проект предназначен для платформы .NET Framework, включить ссылку пакета [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Если проект предназначен для .NET Core, изменения не требуются.

Шаблоны проектов ASP.NET Core 2.x неявно задать `MvcRazorCompileOnPublish` для `true` по умолчанию, что означает этот узел можно безопасно удалить из *.csproj* файла. При желании быть явными, нет никакого вреда параметр `MvcRazorCompileOnPublish` свойства `true`. Следующие *.csproj* образец иллюстрирует этот параметр:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Задать `MvcRazorCompileOnPublish` для `true`и включать ссылку на пакет `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Следующие *.csproj* образец иллюстрирует следующие параметры:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

Подготовка приложения для [развертывания зависит от framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) , выполнив команду, подобную следующей, в корне проекта:

```console
dotnet publish -c Release
```

Объект *< имя_проекта >. PrecompiledViews.dll* файл, содержащий скомпилированный представлений Razor, создается при успешном завершении предварительной компиляции. Например, на следующем снимке экрана показано содержимое *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:

![Представлений Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)
