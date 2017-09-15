---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

> [!NOTE]
> Эти инструкции предназначены для последней версии ASP.NET Core. Хотите приступить к работе с более ранней версией? См. [учебник для версии 1.1](xref:getting-started-1.1).

1. Установите [.NET Core](https://microsoft.com/net/core/).

2. Создайте проект .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. Запустите приложение.

   При необходимости команда `dotnet run` сначала выполняет сборку приложения.

   ```terminal
   dotnet run
   ```

5. Перейдите по адресу `http://localhost:5000`.

### <a name="next-steps"></a>Следующие шаги

Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).

Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).

Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework. Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
