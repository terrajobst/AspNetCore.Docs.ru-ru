---
title: "Начало работы с ASP.NET Core 2.0"
author: rick-anderson
description: "Краткий учебник, в котором с помощью ASP.NET Core создается и запускается простое приложение Hello World."
keywords: "ASP.NET Core, учебник, начало работы"
ms.author: riande
manager: wpickett
ms.date: 08/30/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: f7852f0dddb0585089f5ccd8f4c865f5b87b049b
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2017
---
# <a name="getting-started-with-aspnet-core"></a>Начало работы с ASP.NET Core

> [!NOTE]
> Эти инструкции предназначены для последней версии ASP.NET Core. Хотите приступить к работе с более ранней версией? См. [учебник для версии 1.1](xref:getting-started-1.1).

1. Установите [.NET Core](https://microsoft.com/net/core/).

2. Создайте проект .NET Core.

   В macOS или Linux откройте окно терминала. В Windows откройте окно командной строки.

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
