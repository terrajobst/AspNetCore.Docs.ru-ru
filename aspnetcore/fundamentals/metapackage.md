---
title: "Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x и более поздних версий"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core, вместе с их зависимости."
keywords: ASP.NET Core,NuGet,package,Microsoft.AspNetCore.All,metapackage
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: ff25d80be907994f7ac3d64a8ffa39ae53278ba6
ms.sourcegitcommit: 73bf6b222474d9f1f6aba3feaca4e191069d2121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x

Для этой функции требуется ASP.NET Core 2.x нацеливания .NET Core 2.x.

Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:

* все пакеты, поддерживаемые командой ASP.NET Core;
* все пакеты, поддерживаемые Entity Framework Core; 
* внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core. 

Все возможности ASP.NET Core 2.x и Entity Framework Core 2.x включаются в `Microsoft.AspNetCore.All` пакета. Шаблоны проектов по умолчанию использовать этот пакет.

Номер версии `Microsoft.AspNetCore.All` metapackage представляет версию ASP.NET Core и версии Entity Framework Core (выравнивание с версией .NET Core).

Приложения, использующие `Microsoft.AspNetCore.All` metapackage автоматически воспользоваться преимуществами [хранилище времени выполнения .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store). Хранилище среда выполнения содержит все ресурсы среды выполнения, необходимые для запуска ASP.NET Core 2.x приложений. При использовании `Microsoft.AspNetCore.All` metapackage, **не** активы из упоминаемой пакетов ASP.NET Core NuGet развертываются с приложением &mdash; хранилище среды выполнения .NET Core содержит эти ресурсы. Ресурсы в хранилище среды выполнения компилируются для сокращения времени запуска приложения.

Процесс усечения пакета можно использовать для удаления пакетов, которые не используются. Обрезанная пакеты исключаются в выходных данных для опубликованного приложения.

Следующие *.csproj* ссылки на файлы `Microsoft.AspNetCore.All` metapackage для ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
