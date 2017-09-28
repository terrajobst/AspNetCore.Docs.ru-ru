---
title: "Начало работы с Razor Pages в ASP.NET Core на Mac"
author: rick-anderson
description: "Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio для Mac"
keywords: "ASP.NET Core,Razor Pages,формирование шаблонов,Entity Framework Core,EF,EF Core,база данных,mac,macOS,Visual Studio для Mac"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 9431e8160011d1485925db5cc4f9f83bf7381e97
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Начало работы с Razor Pages в ASP.NET Core с использованием Visual Studio для Mac

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)

В этом учебнике приводятся основные сведения о веб-приложении Razor Pages в ASP.NET Core. Перед работой с этим учебником рекомендуем изучить статью [Введение в Razor Pages](xref:mvc/razor-pages/index). Razor Pages — это рекомендуемый способ создания пользовательского интерфейса для веб-приложений в ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

Установите следующие компоненты:

* [пакет SDK для .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;
* [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a>Создание веб-приложения Razor

Из терминала выполните следующие команды.

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Указанные выше команды используют [интерфейс командной строки .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) для создания и запуска проекта Razor Pages. Откройте в браузере страницу http://localhost:5000 для просмотра приложения.

![Домашняя или индексная страница](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Открытие проекта

Нажмите клавиши CTRL+C, чтобы завершить работу приложения.

В Visual Studio откройте меню **Файл > Открыть файл** и выберите файл *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Запуск приложения

В Visual Studio откройте меню **Выполнить > Запуск без отладки**, чтобы запустить приложение. Visual Studio запускает [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), открывает браузер и переходит к `http://localhost:5000`.

В следующем учебнике мы добавим в проект модель.

>[!div class="step-by-step"]
[Далее: добавление модели](xref:tutorials/razor-pages-mac/model)
