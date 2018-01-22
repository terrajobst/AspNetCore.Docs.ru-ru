---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

> [!NOTE]
> Эти инструкции предназначены для последней версии ASP.NET Core. Хотите приступить к работе с более ранней версией? См. [учебник для версии 1.1](xref:getting-started-1.1).

1. Установите [.NET Core](https://www.microsoft.com/net/core/).

2. Создайте проект .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки. Введите следующую команду:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Запустите приложение.

    Используйте следующие команды для запуска приложения.

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Перейдите по адресу [http://localhost:5000](http://localhost:5000).

6. Откройте файл *Pages/About.cshtml* и измените страницу, чтобы на ней отображалось сообщение "Hello, world! Время на сервере — @DateTime.Now":

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Перейдите по адресу [http://localhost:5000/About](http://localhost:5000/About) и подтвердите изменения.

### <a name="next-steps"></a>Следующие шаги

Учебники по началу работы см. на странице [Учебники по ASP.NET Core](tutorials/index.md).

Сведения об основных понятиях и архитектуре ASP.NET Core см. в статьях [Введение в ASP.NET Core](index.md) и [Основы ASP.NET Core](fundamentals/index.md).

Приложение ASP.NET Core может использовать библиотеку базовых классов и среду выполнения .NET Core или .NET Framework. Дополнительные сведения см. в статье [Выбор между .NET Core и .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
