---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Установите [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Создайте проект .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки. Введите следующую команду:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Запустите приложение с помощью следующих команд:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Перейдите по адресу [http://localhost:5000](http://localhost:5000).

5. Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world! Время на сервере — @DateTime.Now" :

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Установите **установщик пакета SDK** версии 1.0.4 для .NET Core со [страницы "Все загрузки .NET Core"](https://www.microsoft.com/net/download/all).

2. Создайте папку для нового проекта .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Если на компьютере установлена более поздняя версия пакета SDK, создайте файл *global.json*, чтобы выбрать пакет SDK версии 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Создайте проект .NET Core.

   ```terminal
   dotnet new web
   ```

5. Восстановите пакеты.

    ```terminal
    dotnet restore
    ```

6. Запустите приложение.

   ```terminal
   dotnet run
   ```

   При необходимости команда [dotnet run](/dotnet/core/tools/dotnet-run) сначала выполняет сборку приложения.

7. Перейдите по адресу `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end