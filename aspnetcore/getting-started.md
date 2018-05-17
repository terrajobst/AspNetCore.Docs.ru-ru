---
title: Начало работы с ASP.NET Core
author: rick-anderson
description: Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

> [!NOTE]
> Эти инструкции предназначены для последней версии ASP.NET Core. Документ для версии 1.1 см. в разделе [Начало работы с ASP.NET Core 1.1](xref:getting-started-1.1).

1. Установите [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Создайте проект .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки. Введите следующую команду:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Запустите приложение.

    Используйте следующие команды для запуска приложения.

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Перейдите по адресу [http://localhost:5000](http://localhost:5000)

5. Откройте файл <em>Pages/About.cshtml</em> и измените страницу, чтобы на ней отображалось сообщение "Hello, world! Время на сервере — @DateTime.Now":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Перейдите к [http://localhost:5000/About](http://localhost:5000/About) и проверьте изменения.

### <a name="next-steps"></a>Следующие шаги

Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).

Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).

Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework. Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
