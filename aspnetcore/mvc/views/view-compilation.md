---
title: "Компиляция представления Razor и предварительной компиляции"
author: rick-anderson
description: "Ссылки документов о том, как включить компиляции представления MVC Razor и предварительной компиляции в приложениях ASP.NET Core."
keywords: "ASP.NET Core компиляции представления Razor, предварительная компиляция Razor, предварительной компиляции Razor"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 1395717341bfcf5441b78633ca3957630ae5d899
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Компиляция представления Razor и предварительной компиляции в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT)

Представлений Razor компилируются во время выполнения при вызове представления. ASP.NET Core 1.1.0 и более поздней версии можно при необходимости компиляции представлений Razor и развертываются с приложением &mdash; этот процесс называется предварительной компиляции. Предварительная компиляция включает шаблоны проектов ASP.NET Core 2.x по умолчанию.

> [!NOTE]
> Предварительная компиляция представления Razor недоступна, при выполнении [Self-Contained развертывания](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core версии 2.0.0 и более ранних версий.

Замечания по предварительной компиляции.

* Предкомпиляция представлений приведет к более мелкие опубликованный пакет и уменьшение времени запуска.
* После предкомпиляция представлений не может редактировать файлы Razor. Редактируемого представления не будет присутствовать в опубликованный пакет. 

Развертывание предварительно скомпилированного представления:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Если проект предназначен для платформы .NET Framework, включить ссылку пакета `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

Если проект предназначен для .NET Core, изменения не требуются.

Шаблоны проектов ASP.NET Core 2.x неявно задать `MvcRazorCompileOnPublish` для `true` по умолчанию, что означает этот узел можно безопасно удалить из *.csproj* файла. При желании быть явными, нет никакого вреда параметр `MvcRazorCompileOnPublish` свойства `true`. Следующие *.csproj* образец иллюстрирует этот параметр:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Задать `MvcRazorCompileOnPublish` для `true`и включать ссылку на пакет `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`. Следующие *.csproj* образец иллюстрирует следующие параметры:

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---