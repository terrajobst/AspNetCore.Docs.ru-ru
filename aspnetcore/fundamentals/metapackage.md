---
title: "Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x и более поздних версий"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage включает все поддерживаемые пакеты."
keywords: "Пакет ASP.NET Core, NuGet, Microsoft.AspNetCore.All, metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a>Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x

Для этой функции требуется ASP.NET Core 2.x.

[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage для ASP.NET Core включает в себя:

* Все поддерживаемые пакеты командой ASP.NET Core.
* Для всех поддерживаемых пакетов по Entity Framework Core. 
* Зависимости внутренних и сторонних поставщиков, используемые ASP.NET Core и Entity Framework Core. 

Все возможности ASP.NET Core 2.x и Entity Framework Core 2.x включаются в `Microsoft.AspNetCore.All` пакета. Шаблоны проектов по умолчанию использовать этот пакет.

Номер версии `Microsoft.AspNetCore.All` metapackage представляет версию ASP.NET Core и версии Entity Framework Core (выравнивание с версией .NET Core).

Приложения, использующие `Microsoft.AspNetCore.All` metapackage автоматически используют хранилище среды выполнения .NET Core. Хранилище среда выполнения содержит все ресурсы среды выполнения, необходимые для запуска ASP.NET Core 2.x приложений. При использовании `Microsoft.AspNetCore.All` metapackage, **не** активы из упоминаемой пакетов ASP.NET Core NuGet развертываются с приложением &mdash; хранилище среды выполнения .NET Core содержит эти ресурсы. <!-- todo add link to Runtime store -->Ресурсы в хранилище среды выполнения компилируются для сокращения времени запуска приложения.

Процесс усечения пакета можно использовать для удаления пакетов, которые не используются. Обрезанная пакеты исключаются в выходных данных для опубликованного приложения.

Следующие *.csproj* ссылки на файлы `Microsoft.AspNetCore.All` metapackage для ASP.NET Core:

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
