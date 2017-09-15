---
title: "Начало работы с ASP.NET Core 1.1"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core 1.1 создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a>Начало работы с ASP.NET Core 1.1

> [!NOTE]
> Эти инструкции предназначены для ASP.NET Core 1.1. Нужны сведения о последней версии? См. [текущую версию этого учебника](xref:getting-started).

1. Запустите **установщик пакета SDK** версии 1.0.4 для .NET Core, который можно получить на [странице скачивания пакета SDK версии 1.0.4 для .NET Core 1.0.5 и 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Создайте папку для нового проекта .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Создайте проект .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Восстановите пакеты.

    ```terminal
    dotnet restore
    ```

4. Запустите приложение.

   При необходимости команда `dotnet run` сначала выполняет сборку приложения.

   ```terminal
   dotnet run
   ```

5. Перейдите по адресу `http://localhost:5000`.

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Следующие шаги

Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).

Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).

Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework. Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
